# QEMU / KVM virtual machine install

#### Background

- ***Hypervisor*** = also known as a virtual machine monitor (VMM), is a type of
computer software, firmware, or hardware that creates and runs virtual machines. 
 - ***KVM*** =  Kernel-based Virtual Machine, is a hypervisor built into the
   Linux kernel.  KVM is a special operating mode of QEMU that uses CPU
   extensions (HVM) for virtualization via a kernel module.
 - ***QEMU*** =  a generic and open source machine emulator and virtualizer.
   QEMU can use hypervisors includiing KVM to use CPU extensions.
- ***Libvirt*** = is a toolkit to manage virtualization platforms accessible via
  various programming languages.  Among others it manages persistent
  configuration and lifecycle and state management.
 - ***virt-manager*** is a GUI for managing virtual machines through libvirt.

When managing only a handful of VMs for personal use I am leaning towards
thinking that it may be simpler and more transparent not to use libvirt and
virt-manager.

#### Enabling UEFI boot
- See instructions here for copying OVMF_VARS file.  At the moment I am copying
into the same directory as the virtual machine.  Each vm needs its own file:
- https://wiki.archlinux.org/title/QEMU#Booting_in_UEFI_mode


#### Current boot string for Arch installer iso:
- boot order=d specifies to boot from 1st cdrom
- m 8G sets ram of VM.   Default is 128MB which is often to small to boot even
  an installer isp
- machine q35 is recommended chipset for modern hardware
- IOMMU is generic name for intel and AMD virtualisation
- The 2 lines drive if=pflash... enable UEFI boot.  See arch wiki for further
  instruction.


qemu-system-x86_64 \
    -cdrom ~/local_storage_not_backed_up/archiso/archlinux-2026.10.01-x86_64.iso \
    -boot order=d \
    -drive file=/home/charl/local_storage_not_backed_up/virtualmachines/test_qemu/image_file,format=qcow2 \
    -m 8G \
    -enable-kvm \
    -machine q35 \
    -device intel-iommu \
    -drive if=pflash,format=raw,readonly=on,file=/usr/share/edk2/x64/OVMF_CODE.4m.fd \
    -drive if=pflash,format=raw,file=/home/charl/local_storage_not_backed_up/virtualmachines/test_qemu/OVMF_VARS.4m.fd
