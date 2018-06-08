# Virtualization

## Virt Stack
1. **KVM:** Kernel and CPU level, involved with optimization at the low level
2. **QEMU:** Hardware emulation, slightly higher level
3. **LibVirt:** Want to create a simplification to manage VMs without having to go through all the command line junk for every application that wants to talk to QEMU
4. **VirtManager:** User friendly non-commandline management of VMs through LibVirt

#### Interactions
* Virt-Manager wraps a UI around LibVirt  
* LibVirt wraps an API around QEMU  
* QEMU emulates hardware for KVM  
* KVM is at kernel level  

## Cockpit
Cockpit is a web-based linux host management interface  
Headless systems are VMs with no graphical interface  
*Cockpit-Machines* is the virt-plugin for Cockpit, the simple use case is VM management

## Virt Manager
Management of all VMs including creation, starting, stopping, destruction, and editing the configuration.

### Virt Install
The main job of virt-install is to serve as a command line interface which creates the XML code that defines VMs when passed in configuration details.

#### Extending the Virt-Install XML Command Line
1. Create a new branch to isolate the changes
2. Add XMLProperty to the appropriate .py file in `/virt-manager/virtinst/devices`. The main argument to the XMLProperty function is the _XPath_ to the attribute. This could be simple or could involve creating a class and having a ChildXMLProperty.  
There are checkers that can also be defined here to verify that the right information is being passed in, these can be (`is_int`, `is_bool`, and `is_onoff`).
  * add line `mtu_size = XMLProperty("./mtu/@size", is_int=True)` to `interface.py`
3. Add Parser<Device> to `cli.py` (the first argument is the variable created in 1, the second argument should be the key for the command line to parse).  
If you added either `is_bool` or `is_onoff` in the device file, you must add `is_onoff` here as well.
  * add line `ParserNetwork.add_arg("mtu_size","mtu.size")` to `cli.py`
4. Run as simple as a command as you can to verify the XML is being created correctly
  ```
  ./virt-install --connect test:///default --name foo --ram 64 --disk none --pxe --print-xml --<device> <key-value-pair>
  ```
  * the end would be `--network mtu.size=1500`
5. Edit `tests/clitest.py` to an `add_compare` call with the most attributes after that device tag to add an argument with a key-value pair matching what you just created
6. Run `./setup.py test` and check that it failed as expected because of the line you just added and nowhere else
7. Run `./setup.py test --regenerate-output` to regenerate the output that is compared to the output from the regular running of the test so that now they will match

## LibVirt

Most open source linux C projects are built using a **autotools** aka. autoconf  
The key identifying files are `configure.ac` or `configure.in` and `autogen.sh`  

#### Directories
`tools` mainly has the Virsh code  
`src` is most of the source code  
`src/test` has all of the tests  
`src/conf` shared code that essentially parses the XML  
`src/conf/domain_conf.c` parses the XML into a C structure  
`src/qemu` where most of the libvirt work happens  
`src/qemu/qemu_command.c` generates the QEMU command line from the XML parsed in domain_conf.c  

#### Misc 
`ps axwww | grep qemu` will give you the QEMU command line generated in qemu_command.c  
`sudo vi /var/log/libvirt/qemu/f27.log` will give you the log for the VM specified, in this case f27  
`qemuxml2argvtest` is one of the big test files  


#### Building From Source
Run `./autogen.sh` which turns the configure file into a script (and maybe runs it?)
Running `./configure` generates make files from the make file templates  
These scripts require some packages to run. Install these by running these two commands:
  ```
  $  sudo yum builddep libvirt
  $  sudo yum install libtool autoconf
  ```

There are a couple of notable "make" commands:
* `make` just compiles all of the code
* `make check` compiles the code and runs the test suite on it
* `make syntax-check` compiles all the code and checks for variations from coding conventions such as hanging white space etc.

>**Note:**  
NEVER run "sudo make" because then all of the make files will require root permissions.  
Also, NEVER install it, always just run it from source (in this case, the git repo)  

#### Running LibVirt
There is already LibVirt running on the Fedora distribution, so in order to run the version from git you have to stop the process  
`sudo systemctl stop libvirtd` stops the daemon LibVirt process  
(`sudo systemctl start libvirtd` starts the daemon LibVirt process)  

>**Note:**  
`systemd` is the first process run on start up and it starts all of the not user specific background processes (basically all processes ending in 'd')  
`systemctl` keeps tabs on all of them

`sudo ./src/libvirtd` when run from inside the libvirt cloned repo runs the libvirt process from the source code on git  
Once this command has been run, anywhere you run Virsh will be talking to that specific libvirt process  


### Virsh (Virt Shell)
Main use is to start, stop, etc. VMs from a command line interface for the LibVirt API.

To use, run `sudo virsh` from inside the cloned `virt-manager` repo  

#### Connection
The default connection is to `qemu:///system`  
If you run `virsh` without `sudo` you could end up with a different connection which can result in wacky stuff so don't do that   
To check the connection within virsh run the `uri` command  
You can also connect to the test version by running `virsh --connect test:///default`  

#### Commands
`list --all` gives a list of all VMs, their states, and ids  
`start <name>` starts the vm called &lt;name&gt;  
`start <id>` starts the vm with id &lt;id&gt;  
`destroy <name>` destroys the vm called &lt;name&gt;  
`dumpxml <name>` prints out the xml defining the vm called &lt;name&gt;  
&nbsp;&nbsp;&nbsp;&nbsp;`dumpxml <name> > foobar.xml` puts xml into foobar.xml  
`edit <name>` opens the xml defining the vm called &lt;name&gt;  
`undefine <name>` deletes the xml file (?) for the vm called &lt;name&gt;  
`define <xml-file>` creates a new vm from the given xml file (keeping the name of the file (?) )

>**Note:** In most cases, &lt;id&gt; can be substituted for &lt;name&gt; in these VM specific commands  
