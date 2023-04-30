# Arch Linux install and setup notes

Follow latest arch linux install guide per wiki.  Below are notes covering areas that I sometimes get stuck on.

<br>

## Verifying signature of download on Windows
- Download the simple (non-GUI) Windows installer from here and install if not already done and install.  https://gnupg.org/download/index.html

 - 3 Items are required:
    - the iso file (obtained via magnet link or torrent)
    - the iso signature file (link from Arch download page)
    - the public key of the use that signed the iso. This can however be auto-retrieved using gpg command below. 

 - Open cmd and run below command (as per Arch Wiki install instructions Apr 2023) <br>
  gpg --keyserver-options auto-key-retrieve --verify archlinux-version-<blah>.iso.sig <br>
  Note that it is the signature file name included as parameter above, not the iso file name.  Iso needs to have same name though without .sig extension

<br>

## Networking

Internet access is configured when booting from the iso install disk (I have always been able to ping external www.archlinux.org when booted from the install image).  Below is what I have used to get internet working in the live system
 - these steps need to be completed during the install phase when booted from the iso image (when a network connection exists) once "chroot-ed" into the new system
 - install dhcpd
 - List interfaces with ls /sys/class/net or - ip link. 
- Note that lo is the virtual loopback interface and not used in making network connections.
- Enable the daemon with  dhcpcd@interface.service for example systemctl enable dhcpcd@enp0s3.service

<br>

## Arch setup as Virtualbox guest

### SSH
 - Port forwarding needs to be enabled in Virtualbox GUI in order to SSH into virtualbox
 - See relevant sections of below links
 - https://dev.to/developertharun/easy-way-to-ssh-into-virtualbox-machine-any-os-just-x-steps-5d9i
 - https://averagelinuxuser.com/ssh-into-virtualbox/

### Jupyter
 - Port forwarding needs to be enabled Host port  = 8888, Guest Port = 8888
 - Run on the guest with jupyter lab --no-browser --ip="*"
 - See link for guide: https://towardsdatascience.com/how-to-connect-to-jupyterlab-remotely-9180b57c45bb

### Virtualbox networking summary
<img src="./EmbeddedImages/VirtualBoxNetworking.JPG" width=700>
<img src="./EmbeddedImages/VirtualBoxPortForwarding.JPG" width=700>











