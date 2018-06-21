# libvirt-dbus

libvirt-dbus wraps libvirt API to provide a high-level object-oriented API better suited for dbus-based applications.

### Directories
* `data` has the `org.libvirt.Interface.xml` file for every interface
* `src` is all of the source code  
    * contains a `.h` and `.c` file for every interface
    * `gdbus.c` and `connect.c` are key files
* `tests` has the tests

#### Building from Source
1. Run `./autogen.sh` which turns the configure file into a script (and maybe runs it?)

2. Running `./configure` generates make files from the make file templates  
These scripts require some packages to run. Install these by running these two commands:
    ```
    $  sudo yum builddep libvirt-dbus
    ```
    * we created a `configuresystem.sh` script to run `./configure` with the appropriate parameters
3. There are a couple of notable "make" commands:
    * `make` just compiles all of the code
    * `make check` compiles the code and runs the test suite on it
    * `make syntax-check` compiles all the code and checks for variations from coding conventions such as hanging white space etc.

  >**Note:**  
  NEVER run "sudo make" because then all of the make files will require root permissions.  
  Also, NEVER install it, always just run it from source (in this case, the git repo)  

### Adding an Interface

There are three main parts of adding an interface to libvirt-dbus.
