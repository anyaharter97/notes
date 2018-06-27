# Virtualization
* [Projects](virtualization.md#projects)
* [VirtManager Stack](virtualization.md#virtmanager-stack)
* [Virt Shell Stack](virtualization.md#virsh-stack)
* [Cockpit Stack](virtualization.md#cockpit-stack)

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

### Virt Shell Stack
* Virt Shell
	* libvirt
		* QEMU
			* KVM

### Cockpit Stack
* Cockpit Machines
	* libvirt-dbus
		* libvirt
			* QEMU
				* KVM
