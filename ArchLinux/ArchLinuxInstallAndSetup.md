# Arch Linux install and setup notes


## Verifying signature of download on Windows
- Download the simple (non-GUI) Windows installer from here and install if not already done and install.  https://gnupg.org/download/index.html

 - 3 Items are required:
    - the iso file (obtained via magnet link or torrent)
    - the iso signature file (link from Arch download page
    - the public key of the use that signed the iso.  Will only know who signed once teh file is run through gpg as below (https://archlinux.org/master-keys/)
 
 - Open cmd and run gpg --verify [signature file] [iso file]

 - Above may give a message that "Can't check signature: No public key" but will list he signer in which case the public key can be obtained from the link above.

<br>

## Networking



<br>

## Arch setup as Virtualbox guest

 - Port forwarding needs to be enabled in Virtualbox GUI in order to SSH into virtualbox
 - See relevant sections of below links
 - https://dev.to/developertharun/easy-way-to-ssh-into-virtualbox-machine-any-os-just-x-steps-5d9i
 - https://averagelinuxuser.com/ssh-into-virtualbox/


