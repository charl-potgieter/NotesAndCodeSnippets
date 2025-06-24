# QEMU / KVM Virtual Windows 11 Installation


## QUESTIONS ???
 - Do I neeed virt-viewer?
 - For Win 11 can I install qemu-desktop rather than qemu-full?

## Install

qemu-full
virt-manager  (this installs dependency libvirt)
swtpm (QEMU can emulate Trusted Platform Module, which is required by some systems such as Windows 11) 
dnsmasq (for the default NAT/DHCP networking)


## Add user to libvirt group
https://wiki.archlinux.org/title/Libvirt#Using_libvirt_group <br>
sudo gpasswd -a <username> libvirt

## Authenticate with file-based permissions
https://wiki.archlinux.org/title/Libvirt#Authenticate_with_file-based_permissions <br>

Uncomment and amend lines in below file <br>
 /etc/libvirt/libvirtd.conf

```
unix_sock_group = "libvirt"
unix_sock_ro_perms = "0777"  # set to 0770 to deny non-group libvirt users
unix_sock_rw_perms = "0770"
auth_unix_ro = "none"
auth_unix_rw = "none"
```

## Start and enable the daemons
https://wiki.archlinux.org/title/Libvirt#Daemon <br>
sudo start libvirtd.service <br>
sudo enable libvirtd.service


## Other references
https://www.youtube.com/watch?v=G28IVCrKLhI&t=30s 
<br>
https://gitlab.com/stephan-raabe/archinstall/-/blob/main/7-kvm.sh
