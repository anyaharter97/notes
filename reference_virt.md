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

## Reference Documents
* [Cockpit Reference](reference_virt_cockpit.md)
* [LibVirt-DBus Reference](reference_virt_libvirt-dbus.md)
  * [Directories](reference_virt_libvirt-dbus.md#directories)
  * [Building from Source](reference_virt_libvirt-dbus.md#building-from-source)
  * [Running from Source](reference_virt_libvirt-dbus.md#running-from-source)
  * [busctl](reference_virt_libvirt-dbus.md#busctl)
    * [Command Structure](reference_virt_libvirt-dbus.md#command-structure)
    * [Notable OPTIONS](reference_virt_libvirt-dbus.md#notable-options)
    * [Noteable COMMAND](reference_virt_libvirt-dbus.md#noteable-command)
    * [D-Bus Types](reference_virt_libvirt-dbus.md#d-bus-types)
  * [Adding an Interface](reference_virt_libvirt-dbus.md#adding-an-interface)
    * [Introducing the Interface](reference_virt_libvirt-dbus.md#introducing-the-interface)
    * [Properties](reference_virt_libvirt-dbus.md#properties)
    * [Connect Methods](reference_virt_libvirt-dbus.md#connect-methods)
    * [Events](reference_virt_libvirt-dbus.md#events)
    * [Interface Methods](reference_virt_libvirt-dbus.md#interface-methods)
  * [Understanding `gdbus.h` in the Context of Interfaces](reference_virt_libvirt-dbus.md#understanding-gdbush-in-the-context-of-interfaces)
* [LibVirt Reference](reference_virt_libvirt.md)
  * [Directories](reference_virt_libvirt.md#directories)
  * [Misc](reference_virt_libvirt.md#misc)
  * [Building and Running libvirt](reference_virt_libvirt.md#building-and-running-libvirt)
    * [Building from Source](reference_virt_libvirt.md#building-from-source)
    * [Running from Source](reference_virt_libvirt.md#running-from-source)
  * [Adding XML Test Cases](reference_virt_libvirt.md#adding-xml-test-cases)
  * [Comma Escaping](reference_virt_libvirt.md#comma-escaping)
* [Virsh Reference](reference_virt_virsh.md)
  * [Connection](reference_virt_virsh.md#connection)
  * [Commands](reference_virt_virsh.md#commands)
* [Virt Manager Reference](reference_virt_virtmanager.md)
  * [Virt Install](reference_virt_virtmanager.md#virt-install)
    * [Extending the Virt-Install XML Command Line](reference_virt_virtmanager.md#extending-the-virt-install-xml-command-line)
