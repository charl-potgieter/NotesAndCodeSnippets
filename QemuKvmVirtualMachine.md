# QEMU / KVM Virtual Windows 11 Installation


## QUESTIONS ???
 - Do I neeed virt-viewer?
 - For Win 11 can I install qemu-desktop rather than qemu-full?
 - Will this work when network changs? sudo virsh net-autostart default
 - Under setting up virtual machine below.  Is this correct  SATA disk 1 change disk bus to virtio for performance reasons (see youtube video referenced below)
 - seems as if I can allocate 100% of CPU cores to the guest without any adverse impact on the host?  Was allocating all the CPU threads what caused the fan noise to drop or was it just the fact that Win 11 had installed most of the updates?

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
auth_unix_ro = "nonetar
auth_unix_rw = "none"
```

## Configure to use as a normal user without root
 https://wiki.archlinux.org/title/Virt-manager#Non_root_KVM_without_Socket <br>
 Uncomment and replace below with own username and group (which is generally = username) <br>
 /etc/libvirt/qemu.conf <br>
 ``` 
    user = "libvirt-qemu"
    group = "libvirt-qemu"
 ```


## Autostart network
https://wiki.archlinux.org/title/Libvirt#Error_starting_domain:_Requested_operation_is_not_valid <br>
```
    sudo virsh net-autostart default
    virsh net-start default  (this may not be required if a restart is perfomed?)
``` 
The "default": is taken is taken from here: https://www.youtube.com/watch?v=G28IVCrKLhI&t=30s 

## Start and enable the daemons
https://wiki.archlinux.org/title/Libvirt#Daemon <br>
sudo start libvirtd.service <br>
sudo enable libvirtd.service

## Download the Virtio drivers for windows
https://wiki.archlinux.org/title/QEMU#Virtio_drivers_for_Windows <br>
(Download Latest virtio-win ISO)


## Setting up the virtual machine
https://www.youtube.com/watch?v=WmFpwpW6Xko
<br>
Open virtual machine manager and create a new machine.
 - boot options: Enable boot menu, enable cdrom and move to top and click apply
 - SATA disk 1 change disk bus to virtio and click apply -  for performance reasons accoriding to youtube video above.
 - Click add additional hardware (bottom of window), select custom storage and browse to the virtio drivers downloaded above, and fereference type as cdrom
 - Add this device to the bootmenu
 - TPM : Type = emulated, model= CRB, version=2.0 and apply  (does default not work?)
 - Click begin installiation
 - When the message pops up press any key to boot from the cdrom (don't press any key on subsequent reboots though as vritual pc needs to reboot from the hard drive to load the OS)
 - When it comes to install on drive, there are no drives visible.  Click load driver and select AMD64 and w11 (why amd?).  Select this and click install and the drive should now appear
 - Once the windows pc is up and running need to install the spice drivers (guest utils) from inside the windows pc. Click on spice-guest-tools link to download at web page below  https://www.spice-space.org/download.html  <br> Ref  https://wiki.archlinux.org/title/Virt-manager#Guest_utils  
 - seems as if I can allocate 100% of CPU cores to the guest without any adverse impact on the host?  Was allocating all the CPU threads what caused the fan noise to drop or was it just the fact that Win 11 had installed most of the updates?
 - Perform a restart
 - IMPORTANT!!  Navigate to the cd containing the Virtio drivers (as set up above) and run the executable virtio-win-guest-tools.exe.  This makes a huge difference to the font rendering

## Other references
https://www.youtube.com/watch?v=G28IVCrKLhI&t=30s 
<br>
https://gitlab.com/stephan-raabe/archinstall/-/blob/main/7-kvm.sh
<br>
https://www.youtube.com/watch?v=WmFpwpW6Xko
