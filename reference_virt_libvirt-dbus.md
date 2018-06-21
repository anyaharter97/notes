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

There are three main parts of adding an interface to libvirt-dbus: introducing the interface, implementing the properties, and implementing the methods.

We will use NWFilter as an example:

#### Introducing the Interface
1. Create a new file `data/org.libvirt.NWFilter.xml` and add it to `data/Makefile.am`  

  ```xml
  <!DOCTYPE node PUBLIC "-//freedesktop//DTD D-BUS Object Introspection 1.0//EN"
  "http://www.freedesktop.org/standards/dbus/1.0/introspect.dtd">

  <node name="/org/libvirt/nwfilter">
    <interface name="org.libvirt.NWFilter">
    </interface>
  </node>
  ```

2. Create files `src/nwfilter.c` and `src/nwfilter.h` and add them to `src/Makefile.am`

3. In `src/connect.c`, include the interface header file at the top,

  ```c
  #include "nwfilter.h"
  ```
  add a line to free the connection in `virtDBusConnectFree(virtDBusConnect *connect)`,
  ```c
      g_free(connect->nwfilterPath);
  ```
  and then add some lines to `virtDBusConnectNew(virtDBusConnect **connectp, GDBusConnection *bus, const gchar *uri, const gchar *connectPath, GError **error)`


In `src/connect.c`,
  ``` diff
diff --git a/src/connect.c b/src/connect.c
index 0b33bc5..136f7ae 100644
--- a/src/connect.c
+++ b/src/connect.c
```
include the interface header file at the top,
``` diff
@@ -2,6 +2,7 @@
 #include "domain.h"
 #include "events.h"
 #include "network.h"
+#include "nwfilter.h"
 #include "secret.h"
 #include "storagepool.h"
 #include "util.h"
```
add the following line
``` diff
@@ -1394,6 +1395,7 @@ virtDBusConnectFree(virtDBusConnect *connect)

     g_free(connect->domainPath);
     g_free(connect->networkPath);
+    g_free(connect->nwfilterPath);
     g_free(connect->secretPath);
     g_free(connect->storagePoolPath);
     g_free(connect);
```
add the following lines
``` diff
@@ -1451,6 +1453,10 @@ virtDBusConnectNew(virtDBusConnect **connectp,
     if (error && *error)
         return;

+    virtDBusNWFilterRegister(connect, error);
+    if (error && *error)
+        return;
+
     virtDBusSecretRegister(connect, error);
     if (error && *error)
         return;
  ```
