# virsh
* [Connection](virsh.md#connection)
* [Commands](virsh.md#commands)

An interactive shell, and batch scriptable tool for performing management tasks on all libvirt managed domains, networks and storage. This is part of the libvirt core distribution.

Main use is to start, stop, etc. VMs from a command line interface for the libvirt API.

To use, run `sudo virsh` from inside the cloned `virt-manager` or `libvirt` repos.  

### Connection
The default connection is to `qemu:///system`  
If you run `virsh` without `sudo` you could end up with a different connection which can result in wacky stuff so don't do that   
To check the connection within virsh run the `uri` command  
You can also connect to the test version by running `virsh --connect test:///default`  

### Commands
* `list --all` gives a list of all VMs, their states, and ids  
* `start <name>` starts the vm called &lt;name&gt;  
* `start <id>` starts the vm with id &lt;id&gt;  
* `destroy <name>` destroys the vm called &lt;name&gt;  
* `dumpxml <name>` prints out the xml defining the vm called &lt;name&gt;  
  * `dumpxml <name> > foobar.xml` puts xml into foobar.xml  
* `edit <name>` opens the xml defining the vm called &lt;name&gt;  
* `undefine <name>` deletes the xml file (?) for the vm called &lt;name&gt;  
* `define <xml-file>` creates a new vm from the given xml file (keeping the name of the file (?))

>**Note:** In most cases, &lt;id&gt; can be substituted for &lt;name&gt; in these VM specific commands  
