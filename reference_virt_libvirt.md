# LibVirt

Most open source linux C projects are built using a **autotools** aka. autoconf  
The key identifying files are `autogen.sh` and (`configure.ac` or `configure.in`)  

### Directories
* `src` is most of the source code  
  * `src/test` has all of the tests  
  * `src/conf` shared code that essentially parses the XML  
    * `domain_conf.c` parses the XML into a C structure  
  * `src/qemu` where most of the libvirt work happens  
    * `qemu_command.c` generates the QEMU command line from the XML parsed in domain_conf.c  
* `tools` mainly has the Virsh code  

### Misc
* `ps axwww | grep qemu` will give you the QEMU command line generated in qemu_command.c  
* `sudo vi /var/log/libvirt/qemu/f27.log` will give you the log for the VM specified, in this case f27  
* `qemuxml2argvtest.c` is one of the big test files  
* `name-escape.xml` is the file where we test escaping things  
* `VIR_TEST_DEBUG=2 ./tests/qemuxml2argvtest 2>&1 | less` runs the test file with debugging so you can see the full expected and received and then redirects `stderr` output to `stdout` where you can page through it in `less` mode  

### Building and Running LibVirt
#### Building from Source
1. Run `./autogen.sh` which turns the configure file into a script (and maybe runs it?)

2. Running `./configure` generates make files from the make file templates  
These scripts require some packages to run. Install these by running these two commands:
    ```
    $  sudo yum builddep libvirt
    $  sudo yum install libtool autoconf
    ```
    * we created a `configuresystem.sh` script to run `./configure` with the appropriate parameters
3. There are a couple of notable "make" commands:
    * `make` just compiles all of the code
    * `make check` compiles the code and runs the test suite on it
    * `make syntax-check` compiles all the code and checks for variations from coding conventions such as hanging white space etc.

  >**Note:**  
  NEVER run "sudo make" because then all of the make files will require root permissions.  
  Also, NEVER install it, always just run it from source (in this case, the git repo)  

#### Running from Source
1. There is already LibVirt running on the Fedora distribution, so in order to run the version from git you have to stop the process...  
`sudo systemctl stop libvirtd` stops the daemon LibVirt process  
(`sudo systemctl start libvirtd` starts it again)  

    >**Note:**  
    `systemd` is the first process run on start up and it starts all of the not user specific background processes (basically all processes ending in 'd')  
    `systemctl` keeps tabs on all of them

2. `sudo ./src/libvirtd` when run from inside the libvirt cloned repo runs the libvirt process from the source code on git  

3. Once this command has been run, anywhere you run Virsh will be talking to that specific libvirt process  

### Adding XML Test Cases
1. Create a new branch so that all changes are isolated

2. Find an existing test file to modify slightly

3. Edit `tests/qemuxml2argvtest.c` to "DO_TEST" with the <test-name> (and any configurations from the model test file)

4. Edit `tests/qemuxml2xmltest.c` to "DO_TEST" with <test-name> (only if there is a parallel entry for the model test, otherwise ignore steps 7. and 15.)

5. Create file `tests/qemuxml2argvdata/<test-name>.xml` copied from the model test file

6. Create file `tests/qemuxml2argvdata/<test-name>.args`, again copied

7. Create file `tests/qemuxml2xmloutdata/<test-name>.xml`, again copied

8. At this point you should have three sets of files with the same content but different names

9. Now just add the desired xml to the file created in step 3.

10. Run `make check`, `qemuxml2argvtest` (and maybe `qemuxml2xmltest`) should break
    * If the expected test doesn't break, check that you did steps 3. and 4. correctly
    * Other tests such as `virschema` might fail if your xml is invalid

11. Run `VIR_TEST_DEBUG=2 ./tests/qemuxml2argvtest 2>&1 | less` and search for "FAIL". Scroll up to verify that only the desired changes caused the failure
    * If you encounter `QEMU Driver error: unsupported configuration...` then you need to copy the configurations from the old test file under your "DO_TEST" in step 3.

12. Commit all changes so the diffs will be easier to see later

13. Run the command `VIR_TEST_REGENERATE_OUTPUT=1 ./tests/qemuxml2argvtest` to rewrite the `tests/qemuxml2argvdata/<test-name>.args` file to the received output

14. Check the git diffs to verify that nothing unexpected was overwritten during the regeneration of the expected test output

15. Repeat steps 11.-14. for `qemuxml2xmltest` if necessary:
    * Run `VIR_TEST_DEBUG=2 ./tests/qemuxml2xmltest 2>&1 | less` and search for "FAIL". Scroll up to verify that only the desired changes caused the failure
    * Commit all changes so the diffs will be easier to see later
    * Run the command `VIR_TEST_REGENERATE_OUTPUT=1 ./tests/qemuxml2xmltest` to rewrite the `tests/qemuxml2xmloutdata/<test-name>.xml` file to the received output
    * Check the git diffs to verify that nothing unexpected was overwritten during the regeneration of the expected test output

16. Run `make check` one more time to ensure that none of the tests FAIL unexpectedly, `make syntax-check` to ensure no styling errors were introduced, and check the diffs once more to confirm that no expected changes were made

17. Commit the changes and make a patch

### Comma Escaping
1. Create a new branch to isolate all changes

2. In `src/qemu/qemu_command.c`, implement the comma escaping as follows:
    ``` c
    ---    virBufferAsprintf(&opt, "unix:%s", graphics->data.vnc.socket);
    +++    virBufferAddLit(&opt, "unix:");
    +++    qemuBufferEscapeComma(&opt, graphics->data.vnc.socket);
    ```

3. Then, mangle the string following the colon or equals sign so you can find an instance where it goes through the code path above

4. Then `make check` and verify the failure in `qemuxml2argvtest`

5. Run `VIR_TEST_DEBUG=2 ./tests/qemuxml2argvtest 2>&1 | less` from the home directory of the git repository and search for "FAIL"

6. Find your mangled substring and make note of the string around it

7. Also make note of the test that failed, because this is where you will find your XML

8. Open the file in `tests/qemuxml2argvdata/<failed-test-name>.xml` and determine the location of the XML that sets the property you are comma escaping

9. Copy this XML into `tests/qemuxml2argvdata/name-escape.xml`, some of it may not be necessary, but when in doubt, keep it all or risk messing up the schema

10. Remove the mangling in `qemu_command.c` and add a comma to the desired XML property to test comma escaping

11. Run `make check`, and verify that only `qemuxml2argvtest` breaks
    * Other tests such as `virschema` might fail if your xml is invalid

12. Run `VIR_TEST_DEBUG=2 ./tests/qemuxml2argvtest 2>&1 | less` and search for "FAIL". Scroll up to verify that only the desired changes caused the failure and ensure that the comma was escaped as expected
    * If you encounter `QEMU Driver error: unsupported configuration...`, edit `tests/qemuxml2argvtest.c` under "DO_TEST" for "name-escape" copy any configurations from failing test file from step 7., run `make check` again, and repeat 12.

13. Commit all changes so the diffs will be easier to see later

14. Run the command `VIR_TEST_REGENERATE_OUTPUT=1 ./tests/qemuxml2argvtest` to rewrite the `tests/qemuxml2argvdata/name-escape.args` file to the received output

15. Check the git diffs to verify that nothing unexpected was overwritten during the regeneration of the expected test output

16. Run `make check` one more time to ensure that none of the tests FAIL unexpectedly, `make syntax-check` to ensure no styling errors were introduced, and check the diffs once more to confirm that no expected changes were made

17. Commit the changes and make a patch
