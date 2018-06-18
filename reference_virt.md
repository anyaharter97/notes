# Virtualization

## Virt Stack
1. **KVM:** Kernel and CPU level, involved with optimization at the low level
  * KVM is at kernel level  
2. **QEMU:** Hardware emulation, slightly higher level
  * QEMU emulates hardware for KVM  
3. **LibVirt:** Want to create a simplification to manage VMs without having to go through all the command line junk for every application that wants to talk to QEMU
  * LibVirt wraps an API around QEMU  
4. **VirtManager:** User friendly non-commandline management of VMs through LibVirt
  * Virt-Manager wraps a UI around LibVirt  

## Reference Documents
* [Cockpit Reference](reference_virt_cockpit.md)
* [Virt Manager Reference](reference_virt_virtmanager.md)
* [LibVirt Reference](reference_virt_libvirt.md)
* [Virsh Reference](reference_virt_virsh.md)
