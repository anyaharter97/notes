# Virtualization
* [Projects](virtualization.md#projects)
* [virt-manager Stack](virtualization.md#virtmanager-stack)
* [virsh Stack](virtualization.md#virsh-stack)
* [Cockpit Stack](virtualization.md#cockpit-stack)

## Projects
1. **KVM:** Kernel and CPU level, involved with optimization at the low level
    * KVM is at kernel level  
2. **QEMU:** Hardware emulation, slightly higher level
    * QEMU emulates hardware for KVM  
3. **libvirt:** Want to create a simplification to manage VMs without having to go through all the command line junk for every application that wants to talk to QEMU
    * libvirt wraps an API around QEMU  
4. **virt-install:** Creates XML from commandline input
5. **virt-manager:** User friendly non-commandline management of VMs through libvirt
    * virt-manager wraps a UI around libvirt  
6. **virsh:** Command line interface of virt-manager  
7. **libvirt-dbus:** Wraps libvirt API in a dbus format so Cockpit can interact with it more easily
8. **Cockpit:** Web app for virt-manager-type actions

### virt-manager Stack
* virt-manager
	* libvirt
		* QEMU
			* KVM

### virsh Stack
* virsh
	* libvirt
		* QEMU
			* KVM

### Cockpit Stack
* Cockpit Machines
	* libvirt-dbus
		* libvirt
			* QEMU
				* KVM
