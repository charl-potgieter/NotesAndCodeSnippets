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
 - List interfaces with *ls /sys/class/net* or *ip link* 
- Note that lo is the virtual loopback interface and not used in making network connections.
- Enable the daemon with  dhcpcd@interface.service for example systemctl enable dhcpcd@enp0s3.service

<br>


## SSH
 - See Arch Wiki for detail instructions.
 - Recommended to change from 22 to a random higher one
 - Disable passwords and force use of SSH keys (see Arch Wiki for details)
 - If running in VirtualBox -> Port forwarding needs to be enabled in Virtualbox GUI in order to SSH into virtualbox under Machine Settings / Network / Advanced / Port Forwarding.  May need to restart virtual machine after changing these settings.

<br>

This is the example line.



## Jupyter lab remote access over virtualbox
 - Port forwarding needs to be enabled enabled in VirtualBox GUI Host port  = 8888, Guest Port = 8888
 - Will have to run 2 git bash terminals in Windows unless some of below steps are automated in the guest
 - SSH into guest and run following to launch the jupyter lab server:  
 &nbsp; &nbsp; *jupyter lab --no-browser --port=8888*
 - On host (in a new shell) run:   
 &nbsp; &nbsp;*ssh -p \<host ssh port> -L \<host Jupyter Port>:localhost:\<guest jupyter port> \<remote user>@\<remote host ip>*
  - For example if ssh is running on host port 2222 and port forwarding for Jupyter host and guest is 8888 run  
  &nbsp; &nbsp;*ssh -p 2222 -L 8888:localhost:8888 \<user>@\<remote ip address>*
 - open browser on host with url = http://localhost:8888/lab  (on first run a token will be requested which can be copied from output when jupyter lab --no-browser --port=8888 is run)


<br>

## Example virtualbox networking summary
Change ssh guest port 22 to a different number if a non-default port is set as recommended above

<img src="./EmbeddedImages/VirtualBoxNetworking.JPG" width=700>
<img src="./EmbeddedImages/VirtualBoxPortForwarding.JPG" width=700>
