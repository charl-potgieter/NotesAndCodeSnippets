# QEMU / KVM virtual machine install
Install from the command line without virtmanager GUI

## Questions / ToDo
 - Potentially also without libvert (not above depending on outcome)
 - How much of a performace hit will I take using a qcow2 image rather than raw.  Experiment after I have tried a raw image first.

 ## General
  - Most of the below sourced from Archwiki <br>
https://wiki.archlinux.org/title/QEMU#Running_a_virtualized_system
 - I created a qcow2 image rather than raw to enable use of snapshots.
 - At step https://wiki.archlinux.org/title/QEMU#Installing_the_operating_system, note the comment around ram allocated.  I added option -m 4G to get the windows installer to boot
  - Tried https://wiki.archlinux.org/title/QEMU#Booting_in_UEFI_mode as install was not working

