# Virtualization
* [Projects](virtualization.md#projects)
* [virt-manager Stack](virtualization.md#virtmanager-stack)
* [virsh Stack](virtualization.md#virsh-stack)
* [Cockpit Stack](virtualization.md#cockpit-stack)

## Projects
1. **KVM:** Kernel and CPU level, involved with optimization at the low level
2. **QEMU:** Hardware emulation for KVM, slightly higher level
3. **libvirt:** Want to create a simplification to manage VMs without having to go through all the command line junk for every application that wants to talk to QEMU, wraps an API around QEMU, parses XML into a C structure which can be modified in-program
4. **virsh:** Bundled with libvirt, command line libvirt API tool, XML must be passed to virsh for use in libvirt API calls (no XML is created or parsed here)
5. **virt-install:** Bundled with virt-manager, creates XML from commandline input, code is shared with virt-manager
6. **virt-manager:** User friendly non-commandline management of VMs through libvirt, wraps an intelligent UI around libvirt, uses virt-install code to build XML to pass to libvirt API calls from the user input: retrieves XML from libvirt, parses and modifies it using virt-install code, and then sends it back
7. **libvirt-dbus:** Wraps libvirt API in a dbus format so Cockpit can interact with it more easily
8. **Cockpit Machines:** Web app for virt-manager-type actions

### virt-manager Stack
* virt-manager <=> virt-install code
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
