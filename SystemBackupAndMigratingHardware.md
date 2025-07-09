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
     mount /dev/mapper/root /mnt

 - Mount any necessary backup destination drives before chroot (cannot seem to run blkid, fdisk or mount once I am in chroot).  Note the double referenced /mnt - the mount will look someting like this: <br>
 `mount /dev/sdx  /mnt/mnt/ExternalHdd`

 - Use chroot rather than arch-chroot (see reason given on above arch wiki page, although I think my tar exlusions should mitigate this)
	chroot /mnt /bin/bash

 - As sudo, run the backup script which references the exclusion file - this should already be saved on the pc hard drive



## Restoring backup on new hardware

 - be careful - don't install on the orignal machine if not the intention!

 - boot the target machine using arch linux install medium

 - Partition disks on new hardware if required.  Current install contains an EFI boot partition and a dm-crypted root ext 4 partition,
 
 - mount the partitions on the target noting that any encrpyted partition will first need to be opened for example  cryptsetup open /dev/sda2 root && mount /dev/mapper/root /mnt  Mount the unencrpted boot partition at /mnt/boot  (I currently am using seperate partitions - encrypted for root and unencrypted boot as above  which both need to be mounted).  Can refer to /etc/fstab of the source system for guidance.

 - If not repartitioning delete files on old hardware - depending on setup with rm -r /mnt   BE CAREFUL !!! 

 - mount the drive containing the tarball backup (or log into samba or get from cloud).
 
 - Navigate to the /mnt directory (assuming this is the mount point for root on the new target pc)

 - As per above Arch wiki page (with addition of verbose flag) restore the files as follows
	bsdtar --acls --xattrs -xpzvf backupfile
	
- Finalise the installation
	- Follow instructions here <br>
    https://wiki.archlinux.org/title/Migrate_installation_to_new_hardware, in particular note
	- update fstab - first have to delete the old lines referencing the  original partitions (but not the samba partitions or swap file) and then run (before arch-chroot) <br>
    `genfstab -U /mnt >> /mnt/etc/fstab`
	- Arch wiki refers to updating bootloader but in case of sticking with systemd boot I can simply update the root partition (not the boot partition!) referenced in the kernel options of systms d boot in /mnt/boot/loader/entries/arch.conf and /mnt/boot/loader/entries/arch-fallback.conf
	- arch-chroot into the new system and creat a new initramfs with mkinitcpio -P
	- update /etc/hostname
	- Refer paths excluded from backup per exclusion file due to senstive nature.  These may need to be manually restored by reference to data stored in a password keeper.
