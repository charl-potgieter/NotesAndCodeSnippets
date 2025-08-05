# System backup and restore

## References
 - https://wiki.archlinux.org/title/Migrate_installation_to_new_hardware  (The top to bottom approach)
 - https://wiki.archlinux.org/title/Installation_guide#Example_layouts
 - https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system
 - https://wiki.archlinux.org/title/Full_system_backup_with_tar
 - https://wiki.archlinux.org/title/Rsync#Full_system_backup  (for guidance as to recommended exclusion folders)
 - https://wiki.archlinux.org/title/Systemd-boot


## Backing up the system
- I am currently backing up using bsdtar backup as permissions are maintained inside the tarball irrespective of type of filesystem where the tarball is saved.  Note, bsdtar is used as GNU tar does not preserve extended attributes as noted on Arch Wiki.

- Follow instructions as per arch wiki
	https://wiki.archlinux.org/title/Full_system_backup_with_tar

In summary
 - Ensure that my custom tar backup script and exclusion file are located on the pc to be backed up.  These were constructed based on arch wiki page above.

 - Consider any sensitve information that may exist on the system that may need to be added to the exclusion file, and potentially add it to a password keeper to enable it to be manually added to the new pc.

 - Do not backup a live system, boot pc using arch installation usb.

- Open any encrypted drives e.g. <br> `cryptsetup open /dev/sdx root.`

 - Mount necessary partitions - may include a root and a boot partition.  If mounting an encrypted partition it may need to be mounted with something like this:
    `mount /dev/mapper/root /mnt`

 - Mount any necessary backup destination drives before chroot (cannot seem to run blkid, fdisk or mount once I am in chroot).  Note the double referenced /mnt - the mount will look someting like this: <br>
 `mount /dev/sdx  /mnt/mnt/ExternalHdd`

 - Use chroot rather than arch-chroot (see reason given on above arch wiki page, although I think my tar exlusions should mitigate this)
	chroot /mnt /bin/bash

 - As sudo, run the backup script which references the exclusion file - this should already be saved on the pc hard drive



## Restoring backup on new hardware

 - Backups can also be restored on a virtual machine for testing purposes
 
 - Safest option is to copy backup tarball onto a usb stick with nothing else important on it.

 - Be careful - don't install on the orignal machine if not the intention!

 - Boot the target machine using arch linux install medium

 - Partition disks on new hardware if required.  Current install contains an EFI boot partition and a dm-crypted root ext 4 partition,
 
 - Mount the partitions on the target noting that any encrpyted partition will first need to be opened for example <br>
 `cryptsetup open /dev/sdx root` <br>
 `mount /dev/mapper/root /mnt` <br>
 Mount the unencrpted boot partition at /mnt/boot  (I currently am using seperate partitions - encrypted for root and unencrypted boot as above  which both need to be mounted).  Can refer to /etc/fstab of the source system for guidance as to the partitioning scheme being utilised.

 - If not repartitioning delete files on old hardware - depending on setup with rm -r /mnt   BE CAREFUL - maybe safer to do this before mounting the drive containing the backup !!! 

 - mount the drive containing the tarball backup (or log into samba or get from cloud).
 
 - Navigate to the /mnt directory (assuming this is the mount point for root on the new target pc)

 - As per above Arch wiki page (with addition of verbose flag) restore the files as follows <br>
	`bsdtar --acls --xattrs -xpzvf backupfile`
	
- Finalise the installation
	- Follow instructions here <br>
    https://wiki.archlinux.org/title/Migrate_installation_to_new_hardware, in particular note
	- update fstab - first have to delete the old lines referencing the  original partitions (but not the samba partitions or swap file) and then run (before arch-chroot) <br>
    `genfstab -U /mnt >> /mnt/etc/fstab`
	- Arch wiki refers to updating bootloader but in case of sticking with systemd boot I can simply update the root partition (not the boot partition!) referenced in the kernel options of systms d boot in /mnt/boot/loader/entries/arch.conf and /mnt/boot/loader/entries/arch-fallback.conf (Note that the UUID that needs to be included in these files is that which appears per BLKID, which is different to /etc/fstab)
	- arch-chroot into the new system and creat a new initramfs with mkinitcpio -P
	- update /etc/hostname
	- Refer paths excluded from backup per exclusion file due to senstive nature.  These may need to be manually restored by reference to data stored in a password keeper.

## Restoring on VirtualBox

- When creating virtual machine ensure enable EFI is ticked under hardware if the operating system is running in EFI 
- Make hard drive sufficient size to accomodate OS as well as the tarball to be extracted
- Ensure OS type is selected as Linux->Arch Linux.  This has a habit of switching to something else while completing settings so ensure the blue Arch logo is displayed on the vitrtual machine (this can be set under Settings / General)
- Use scp to copy the system backup tarball onto the virtual machine (Share folders are challenging as machine  is not yet set up so guest additions can't easily be installed.  Converting the tarball to an iso is challenging due to tarball size)
- Follow below approach in order to enable ssh access on the virtual machine:
    - It appears as if openssh is already installed on the arch iso so no need to re-install
    - systemctl start sshd
    - `passwd root` sets root password on iso as ssh does not work without password
    - Set virtualbox network adaptor as NAT (should be the default)
    - setup port forwarding in Virtualbox gui with below values: <br>
    `Name: SSH`<br>
`Protocol: TCP`<br>
`Host IP: 127.0.0.1`<br>
`Host Port: 2222`<br>
`Guest IP: (leave blank)`<br>
`Guest Port: 22`<br>
    - Should now be able to copy from guest to host with : <br>
    `scp -P 2222 /path/to/arch-backup.tar.gz root@localhost:/mnt`
    - Note destination path should be on the mounted virtual hard drive, dont try copying to the mounted install iso
 - The GUI tends to freeze when extracting the tarball.  The preview pane in the virtualbox manager still seems to update.  Maybe wait until it has no more activity?  It may be better to SSH into the virtual machine and run the extraction via SSH from the host?
 - Sway seems to work with VBOXVGA graphics card without 3d acceleration enabled, even though it gets flagged as not a recommended setting by tthe virtualbox GUI.  The recommended graphhics card of VMSVGA does not seem to work but maybe that is because I have not yet attempted to install guest additions.
 - Consider deleting the tarball in the VM to save drive space
