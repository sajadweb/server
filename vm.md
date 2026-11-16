vm
 
virt-install \
  --name k8s-worker-02 \
  --memory 2048 \
  --vcpus 2 \
  --cpu host-passthrough \
  --disk pool=k8s-lab,size=25,format=qcow2,bus=virtio \
  --cdrom /var/lib/libvirt/boot/ubuntu-24.04.5-live-server-amd64.iso \
  --network network=k8s-net,model=virtio \
  --os-variant ubuntu24.04 \
  --graphics spice \
  --video virtio \
  --noautoconsole
 
```عالیه! تست‌های Worker-01 همگی موفق هستند. شبکه، Gateway، اینترنت و DNS بدون مشکل کار می‌کنند.
Worker-01 تأیید شد
نتیجه بررسی Worker-01
مشخصه	مقدار	وضعیت
Hostname	k8s-worker-01	OK
IP	192.168.250.21	OK
Gateway	192.168.250.1	OK
Internet	Ping موفق	OK
DNS	IPv4 موفق	OK
SSH	متصل	OK

مرحله 04.19 — ساخت Worker-02
حالا آخرین ماشین مجازی را می‌سازیم.
1. خروج از SSH
اگر هنوز داخل Worker-01 هستی:
exit


باید به ترمینال Ubuntu اصلی برگردی:
sajjad@sajjad-afra:~$


2. ساخت VM
این دستور را روی Ubuntu اصلی اجرا کن:
virt-install \
  --name k8s-worker-02 \
  --memory 2048 \
  --vcpus 2 \
  --cpu host-passthrough \
  --disk pool=k8s-lab,size=25,format=qcow2,bus=virtio \
  --cdrom /var/lib/libvirt/boot/ubuntu-24.04.5-live-server-amd64.iso \
  --network network=k8s-net,model=virtio \
  --os-variant ubuntu24.04 \
  --graphics spice \
  --video virtio \
  --noautoconsole


3. تنظیمات نصب Ubuntu
در Virt-Manager ماشین k8s-worker-02 را باز کن.
گزینه	مقدار
Hostname	k8s-worker-02
Username	worker2
CPU	2
RAM	2GB
Disk	25GB
OpenSSH	فعال
Proxy	خالی

4. تنظیم شبکه
در صفحه IPv4، گزینه Manual را انتخاب کن و مقادیر را دقیقاً به ترتیب زیر وارد کن:
Worker-02 Network Configuration
فیلد	مقدار
IPv4 Method	Manual
Subnet	192.168.250.0/24
Address	192.168.250.22
Gateway	192.168.250.1
Name servers	1.1.1.1, 8.8.8.8
Search domains	خالی


Hostname را حتماً k8s-worker-02 قرار بده.
5. اتصال SSH
بعد از نصب، از Ubuntu اصلی اجرا کن:
ssh worker2@192.168.250.22


6. تست شبکه
داخل Worker-02 اجرا کن:
hostnamectl
ip -br addr
ip route

ping -c 4 192.168.250.1
ping -c 4 8.8.8.8

getent ahostsv4 archive.ubuntu.com



وضعیت پروژه
ساخت زیرساخت Local
4/5



- Ready
   CP-01
- Ready
   CP-02
- Ready
   CP-03
- Ready
   Worker-01
- Pending
   Worker-02

بعد از ساخت Worker-02، یک مرحله مهم داریم: تست ارتباط تمام پنج Node با یکدیگر، تنظیم SSH و آماده‌سازی نهایی کلاستر قبل از نصب Kubernetes.
