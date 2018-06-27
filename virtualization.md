# Virtualization

## Projects
1. **KVM:** Kernel and CPU level, involved with optimization at the low level
    * KVM is at kernel level  
2. **QEMU:** Hardware emulation, slightly higher level
    * QEMU emulates hardware for KVM  
3. **LibVirt:** Want to create a simplification to manage VMs without having to go through all the command line junk for every application that wants to talk to QEMU
    * LibVirt wraps an API around QEMU  
4. **VirtInstall:** Creates XML from commandline input
5. **VirtManager:** User friendly non-commandline management of VMs through LibVirt
    * Virt-Manager wraps a UI around LibVirt  
6. **Virsh:** Command line interface of VirtManager  
7. **Cockpit:** Web app for VirtManager-type actions

### VirtManager Stack
* VirtManager
	* libvirt
		* QEMU
			* KVM

### Virsh Stack
* Virsh
	* libvirt
		* QEMU
			* KVM

### Cockpit Stack
* Cockpit Machines
	* libvirt-dbus
		* libvirt
			* QEMU
				* KVM
