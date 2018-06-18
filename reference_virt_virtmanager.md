# Virt Manager
Management of all VMs including creation, starting, stopping, destruction, and editing the configuration.

## Virt Install
The main job of virt-install is to serve as a command line interface which creates the XML code that defines VMs when passed in configuration details.

### Extending the Virt-Install XML Command Line
1. Create a new branch to isolate the changes

2. Add XMLProperty to the appropriate .py file in `/virt-manager/virtinst/devices`. The main argument to the XMLProperty function is the _XPath_ to the attribute. This could be simple or could involve creating a class and having a ChildXMLProperty.  
There are checkers that can also be defined here to verify that the right information is being passed in, these can be (`is_int`, `is_bool`, and `is_onoff`).
    * add line `mtu_size = XMLProperty("./mtu/@size", is_int=True)` to `interface.py`

3. Add Parser<Device> to `cli.py` (the first argument is the variable created in 1, the second argument should be the key for the command line to parse).  
If you added either `is_bool` or `is_onoff` in the device file, you must add `is_onoff` here as well.
    * add line `ParserNetwork.add_arg("mtu_size","mtu.size")` to `cli.py`

4. Run as simple as a command as you can to verify the XML is being created correctly, for example:
  `./virt-install --connect test:///default --name foo --ram 64 --disk none --pxe --print-xml --<device> <key-value-pair>`. 

    * the end would be `--network mtu.size=1500`

5. Edit `tests/clitest.py` to an `add_compare` call with the most attributes after that device tag to add an argument with a key-value pair matching what you just created

6. Run `./setup.py test` and check that it failed as expected because of the line you just added and nowhere else

7. Run `./setup.py test --regenerate-output` to regenerate the output that is compared to the output from the regular running of the test so that now they will match

8. Run `./setup.py pylint` to ensure that no style issues were introduced.
