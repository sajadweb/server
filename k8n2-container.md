عالیه. از اینجا به بعد سرعت کار را بیشتر می‌کنیم. موافقم که لازم نیست برای هر بخش تست‌های تکراری و طولانی انجام بدهیم.

مرحله 05.6 — تکمیل شد

نتیجه بررسی Firewall:

| Node      | UFW      |
| --------- | -------- |
| CP-01     | Inactive |
| CP-02     | Inactive |
| CP-03     | Inactive |
| Worker-01 | Inactive |
| Worker-02 | Inactive |

فعلاً نیازی به باز کردن پورت یا تغییر Firewall نداریم. در صورت بروز مشکل ارتباطی، سراغ بررسی UFW و قوانین `nftables` می‌رویم.

## وضعیت فعلی

- پنج VM ساخته شده‌اند.
- IP ثابت هر پنج Node تنظیم شده.
- ارتباط بین تمام Nodeها برقرار است.
- SSH Key بدون Password کار می‌کند.
- DNS و NTP فعال هستند.
- Hostnameها و Machine IDها یکتا هستند.

مرحله 05 را از نظر زیرساخت شبکه تکمیل‌شده در نظر می‌گیریم.

خروجی Firewall هم کاملاً مشخص است؛ هر پنج Node وضعیت `inactive` دارند. برای Lab فعلاً Firewall را دست‌کاری نمی‌کنیم و اگر بعداً خطای ارتباطی داشتیم بررسی‌اش می‌کنیم.

از اینجا وارد نصب واقعی Kubernetes می‌شویم.

# مرحله 06 — نصب و پیکربندی Container Runtime

شروع مرحله 06

ابتدا `containerd` را روی هر پنج Node نصب می‌کنیم.

چرا containerd؟

Kubernetes برای اجرای Containerهای داخل Pod به یک Runtime نیاز دارد. ما containerd را انتخاب کردیم چون مستقیماً از CRI پشتیبانی می‌کند و نیازی به Docker Engine روی Nodeهای Kubernetes نداریم.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://kubernetes.io\&sz=32)

Kubernetes

+1



تصمیم فنی:

- Package: `containerd` از مخزن رسمی Ubuntu
- Runtime: containerd 2.x
- Cgroup Driver: systemd
- CRI: فعال
- Docker: فقط روی Host برای Build کردن Imageها باقی می‌ماند.

مستندات Ubuntu برای 24.04 بسته containerd نسخه 2.2.x را ارائه می‌کنند.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://packages.ubuntu.com\&sz=32)

Details of package containerd in noble

+1



## 06.1 — نصب containerd روی پنج Node

برای سرعت، یک دستور از Host اجرا می‌کنیم که به هر پنج VM وصل شود و نصب را انجام دهد.

این دستور را در Ubuntu اصلی (Host) اجرا کن:

```
for target in \
  afra@192.168.250.11 \
  afra2@192.168.250.12 \
  afra3@192.168.250.13 \
  worker1@192.168.250.21 \
  worker2@192.168.250.22
do
  echo
  echo "========== $target =========="

  if ! ssh -t \
    -i ~/.ssh/k8s_lab_ed25519 \
    -o IdentitiesOnly=yes \
    "$target" '
      set -e

      sudo apt-get update

      sudo apt-get install -y containerd

      sudo mkdir -p /etc/containerd

      containerd config default | \
        sudo tee /etc/containerd/config.toml >/dev/null

      sudo sed -i \
        "s/SystemdCgroup = false/SystemdCgroup = true/g" \
        /etc/containerd/config.toml

      sudo systemctl enable --now containerd

      sudo systemctl restart containerd

      echo "===== VERSION ====="
      containerd --version

      echo "===== SERVICE ====="
      sudo systemctl is-active containerd

      echo "===== CGROUP ====="
      sudo grep -n SystemdCgroup /etc/containerd/config.toml

      echo "===== DISABLED PLUGINS ====="
      sudo grep -n disabled_plugins /etc/containerd/config.toml
    '
  then
    echo "FAILED: $target"
    break
  fi
done
```

برای هر Node رمز `sudo` همان ماشین را وارد کن.

### 06.2 — چرا SystemdCgroup را فعال کردیم؟

این تنظیم بسیار مهم است:

```
SystemdCgroup = true
```

Kubernetes و containerd باید از یک روش مدیریت منابع CPU و RAM استفاده کنند.

در Ubuntu ما از systemd استفاده می‌کنیم؛ بنابراین این تنظیم باعث هماهنگی مدیریت منابع بین Kubelet و Runtime می‌شود.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://kubernetes.io\&sz=32)

Kubernetes



همچنین مطمئن می‌شویم CRI در تنظیمات containerd غیرفعال نشده باشد.

### خروجی مورد انتظار

برای هر پنج Node باید چیزی شبیه این ببینی:

```
containerd --version
containerd github.com/containerd/containerd/v2 ...

systemctl is-active containerd
active

SystemdCgroup = true
```

و `disabled_plugins` نباید شامل `cri` باشد.

فعلاً فقط همین مرحله را انجام بده.

بعد از تأیید containerd، مستقیماً می‌رویم سراغ:

- 07 — نصب kubeadm، kubelet و kubectl
- 08 — راه‌اندازی Control Plane
- 09 — نصب CNI و شبکه Kubernetes
- 10 — اتصال Workerها

ز این به بعد تست‌های تکراری را حذف می‌کنیم و فقط تست‌های ضروری هر مرحله را انجام می‌دهیم.


عالیه! نصب containerd روی هر پنج Node با موفقیت انجام شده است.
مرحله 06 — تکمیل شد
نتیجه نصب
Node	Containerd	Version	Cgroup
CP-01	Active	2.2.1	systemd
CP-02	Active	2.2.1	systemd
CP-03	Active	2.2.1	systemd
Worker-01	Active	2.2.1	systemd
Worker-02	Active	2.2.1	systemd

همچنین disabled_plugins = [] است؛ یعنی CRI در تنظیمات غیرفعال نشده است. 
Pasted text(20261116-141201).txt
 Pasted text(20261116-141201).txt
 Pasted text(20261116-141201).txt
 Pasted text(20261116-141201).txt
 Pasted text(20261116-141201).txt

حالا وارد نصب ابزارهای اصلی Kubernetes می‌شویم.
مرحله 07 — نصب Kubernetes Components
طبق مستندات رسمی Kubernetes، نسخه پایدار فعلی برای نصب ما Kubernetes v1.37 است. پیشنهاد می‌کنم همین نسخه را روی هر پنج Node نصب کنیم تا تمام اجزای کلاستر یک نسخه یکسان داشته باشند. 


Kubernetes
+1



سه ابزار اصلی نصب می‌شوند:
ابزار	کاربرد
kubeadm	نصب و راه‌اندازی کلاستر
kubelet	اجرای Podها روی Node
kubectl	مدیریت و کنترل Kubernetes

نصب را روی هر پنج Node انجام می‌دهیم؛ اما قبل از آن یک آماده‌سازی ضروری داریم.
07.1 — آماده‌سازی Kernel و Swap
این تنظیمات برای اجرای صحیح Kubernetes لازم‌اند:
- غیرفعال کردن Swap
- فعال‌سازی overlay
- فعال‌سازی br_netfilter
- فعال‌سازی IP Forwarding
- فعال‌سازی پردازش ترافیک Bridge توسط iptables
این تنظیمات را روی هر پنج Node اعمال می‌کنیم. 


Kubernetes
+1



اجرای آماده‌سازی روی پنج Node
این دستور را روی Ubuntu اصلی (Host) اجرا کن:
for target in \
  afra@192.168.250.11 \
  afra2@192.168.250.12 \
  afra3@192.168.250.13 \
  worker1@192.168.250.21 \
  worker2@192.168.250.22
do
  echo
  echo "========== $target =========="

  ssh -t \
    -i ~/.ssh/k8s_lab_ed25519 \
    -o IdentitiesOnly=yes \
    "$target" '
      set -e
      sudo -v

      # Disable Swap
      sudo swapoff -a
      sudo sed -ri \
        "/^[[:space:]]*[^#].*[[:space:]]swap[[:space:]]/s/^/#/" \
        /etc/fstab

      # Load Kernel Modules
      printf "overlay\nbr_netfilter\n" | \
        sudo tee /etc/modules-load.d/k8s.conf

      sudo modprobe overlay
      sudo modprobe br_netfilter

      # Configure Sysctl
      sudo tee /etc/sysctl.d/99-kubernetes-cri.conf <<EOF
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

      sudo sysctl --system >/dev/null

      echo "===== SWAP ====="
      sudo swapon --show

      echo "===== MODULES ====="
      lsmod | grep -E "^(overlay|br_netfilter)"

      echo "===== SYSCTL ====="
      sysctl net.bridge.bridge-nf-call-iptables
      sysctl net.bridge.bridge-nf-call-ip6tables
      sysctl net.ipv4.ip_forward

      echo "===== DONE ====="
    '

  if [ $? -ne 0 ]; then
    echo "FAILED: $target"
    break
  fi
done


این عملیات فقط روی پنج VM انجام می‌شود و به Ubuntu اصلی و Docker دست نمی‌زند.
خروجی مورد انتظار
بررسی	مقدار
Swap	غیرفعال
overlay	Loaded
br_netfilter	Loaded
bridge-nf-call-iptables	1
bridge-nf-call-ip6tables	1
ip_forward	1

وقتی این مرحله موفق شد، مستقیماً دستور نصب kubeadm، kubelet و kubectl نسخه 1.37 را روی هر پنج Node اجرا می‌کنیم.

فعلاً فقط همین دستور را اجرا کن و خروجی را بفرست.
