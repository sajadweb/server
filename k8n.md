```bash
mkdir -p ~/k8s-lab/network

nano ~/k8s-lab/network/k8s-net.xml


sudo virsh pool-define-as \
  k8s-lab \
  dir \
  --target /var/lib/libvirt/images/k8s-lab
Pool k8s-lab defined

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-build k8s-lab
Pool k8s-lab built

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-autostart k8s-lab
Pool k8s-lab marked as autostarted

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-list --all
 Name      State      Autostart
---------------------------------
 default   active     yes
 k8s-lab   inactive   yes

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-info k8s-lab
Name:           k8s-lab
UUID:           29dca130-1193-4155-87bf-c31efa03343d
State:          inactive
Persistent:     yes
Autostart:      yes

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-list --all
 Name      State      Autostart
---------------------------------
 default   active     yes
 k8s-lab   inactive   yes

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-info k8s-lab
Name:           k8s-lab
UUID:           29dca130-1193-4155-87bf-c31efa03343d
State:          inactive
Persistent:     yes
Autostart:      yes

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-start k8s-lab
Pool k8s-lab started

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-list --all
 Name      State    Autostart
-------------------------------
 default   active   yes
 k8s-lab   active   yes

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh pool-info k8s-lab
Name:           k8s-lab
UUID:           29dca130-1193-4155-87bf-c31efa03343d
State:          running
Persistent:     yes
Autostart:      yes
Capacity:       232.64 GiB
Allocation:     72.45 GiB
Available:      160.19 GiB

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo ls -ld /var/lib/libvirt/images/k8s-lab
drwxr-xr-x 2 root root 4096 Nov 16 13:25 /var/lib/libvirt/images/k8s-lab
sajjad@sajjad-afra:~/k8s-lab/iso$ mkdir -p ~/k8s-lab/network
sajjad@sajjad-afra:~/k8s-lab/iso$ nano ~/k8s-lab/network/k8s-net.xml
sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh net-define ~/k8s-lab/network/k8s-net.xml
Network k8s-net defined from /home/sajjad/k8s-lab/network/k8s-net.xml

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh net-start k8s-net
Network k8s-net started

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh net-start k8s-net
error: Failed to start network k8s-net
error: Requested operation is not valid: network is already active

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh net-autostart k8s-net
Network k8s-net marked as autostarted

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh net-list --all
 Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes
 k8s-net   active   yes         yes

sajjad@sajjad-afra:~/k8s-lab/iso$ sudo virsh net-info k8s-net
Name:           k8s-net
UUID:           ebfbab43-5e2c-4d82-bf48-d75ae12abdc1
Active:         yes
Persistent:     yes
Autostart:      yes
Bridge:         virbr-k8s

sajjad@sajjad-afra:~/k8s-lab/iso$ ip -br addr show virbr-k8s
virbr-k8s        DOWN           192.168.250.1/24 
sajjad@sajjad-afra:~/k8s-lab/iso$ 
```
