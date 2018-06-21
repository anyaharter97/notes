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

    ``` xml
    <!DOCTYPE node PUBLIC "-//freedesktop//DTD D-BUS Object Introspection 1.0//EN"
    "http://www.freedesktop.org/standards/dbus/1.0/introspect.dtd">

    <node name="/org/libvirt/nwfilter">
      <interface name="org.libvirt.NWFilter">
      </interface>
    </node>
    ```

2. Create files `src/nwfilter.c` and `src/nwfilter.h` and add them to `src/Makefile.am`

3. The contents of `src/nwfilter.h` should look like this:

    ``` c
    #pragma once

    #include "connect.h"

    #define VIRT_DBUS_NWFILTER_INTERFACE "org.libvirt.NWFilter"

    void
    virtDBusNWFilterRegister(virtDBusConnect *connect,
                             GError **error);
    ```

4. The contents of `src/nwfilter.c` should look like this:

    ``` c
    #include "nwfilter.h"
    #include "util.h"

    #include <libvirt/libvirt.h>

    static virNWFilterPtr
    virtDBusNWFilterGetVirNWFilter(virtDBusConnect *connect,
                                   const gchar *objectPath,
                                   GError **error)
    {
        virNWFilterPtr nwfilter;

        if (virtDBusConnectOpen(connect, error) < 0)
            return NULL;

        nwfilter = virtDBusUtilVirNWFilterFromBusPath(connect->connection,
                                                      objectPath,
                                                      connect->nwfilterPath);
        if (!nwfilter) {
            virtDBusUtilSetLastVirtError(error);
            return NULL;
        }

        return nwfilter;
    }

    static virtDBusGDBusPropertyTable virtDBusNWFilterPropertyTable[] = {
        { 0 }
    };

    static virtDBusGDBusMethodTable virtDBusNWFilterMethodTable[] = {
        { 0 }
    };

    static gchar **
    virtDBusNWFilterEnumerate(gpointer userData)
    {
        virtDBusConnect *connect = userData;
        g_autoptr(virNWFilterPtr) nwfilters = NULL;
        gint num = 0;
        gchar **ret = NULL;

        if (!virtDBusConnectOpen(connect, NULL))
            return NULL;

        num = virConnectListAllNWFilters(connect->connection, &nwfilters, 0);
        if (num < 0)
            return NULL;

        if (num == 0)
            return NULL;

        ret = g_new0(gchar *, num + 1);

        for (gint i = 0; i < num; i++) {
            ret[i] = virtDBusUtilBusPathForVirNWFilter(nwfilters[i],
                                                       connect->nwfilterPath);
        }

        return ret;
    }

    static GDBusInterfaceInfo *interfaceInfo;

    void
    virtDBusNWFilterRegister(virtDBusConnect *connect,
                             GError **error)
    {
        connect->nwfilterPath = g_strdup_printf("%s/nwfilter",
                                                connect->connectPath);

        if (!interfaceInfo) {
            interfaceInfo = virtDBusGDBusLoadIntrospectData(VIRT_DBUS_NWFILTER_INTERFACE,
                                                            error);
            if (!interfaceInfo)
                return;
        }

        virtDBusGDBusRegisterSubtree(connect->bus,
                                     connect->nwfilterPath,
                                     interfaceInfo,
                                     virtDBusNWFilterEnumerate,
                                     virtDBusNWFilterMethodTable,
                                     virtDBusNWFilterPropertyTable,
                                     connect);
    }
    ```

    * It appears that the default identifier used here is the UUID. In the case that there is no UUID, another unique identifier with a lookup method is used

5. In `src/connect.c`, mirror the following changes:

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

    ``` diff
    @@ -1394,6 +1395,7 @@ virtDBusConnectFree(virtDBusConnect *connect)

         g_free(connect->domainPath);
         g_free(connect->networkPath);
    +    g_free(connect->nwfilterPath);
         g_free(connect->secretPath);
         g_free(connect->storagePoolPath);
         g_free(connect);
    ```

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

6. In `src/connect.h`, mirror the following changes:

    ``` diff
    @@ -14,6 +14,7 @@ struct virtDBusConnect {
         const gchar *connectPath;
         gchar *domainPath;
         gchar *networkPath;
    +    gchar *nwfilterPath;
         gchar *secretPath;
         gchar *storagePoolPath;
         virConnectPtr connection;
    ```

7. In `src/util.h`, declare the following five functions (this block of code occurs in a series of similar blocks, one for each interface, in alphabetical order):

    ``` c
    virNWFilterPtr
    virtDBusUtilVirNWFilterFromBusPath(virConnectPtr connection,
                                       const gchar *path,
                                       const gchar *nwfilterPath);

    gchar *
    virtDBusUtilBusPathForVirNWFilter(virNWFilterPtr nwfilter,
                                      const gchar *nwfilterPath);

    void
    virtDBusUtilVirNWFilterListFree(virNWFilterPtr *nwfilters);

    G_DEFINE_AUTOPTR_CLEANUP_FUNC(virNWFilter, virNWFilterFree);
    G_DEFINE_AUTOPTR_CLEANUP_FUNC(virNWFilterPtr, virtDBusUtilVirNWFilterListFree);
    ```

8. In `src/util.c`, define the following three functions (this block of code occurs in a series of similar blocks, one for each interface, in alphabetical order):

    ``` c
    virNWFilterPtr
    virtDBusUtilVirNWFilterFromBusPath(virConnectPtr connection,
                                       const gchar *path,
                                       const gchar *nwfilterPath)
    {
        g_autofree gchar *name = NULL;
        gsize prefixLen = strlen(nwfilterPath) + 1;

        name = virtDBusUtilDecodeUUID(path + prefixLen);

        return virNWFilterLookupByUUIDString(connection, name);
    }

    gchar *
    virtDBusUtilBusPathForVirNWFilter(virNWFilterPtr nwfilter,
                                      const gchar *nwfilterPath)
    {
        gchar uuid[VIR_UUID_STRING_BUFLEN] = "";
        g_autofree gchar *newUuid = NULL;
        virNWFilterGetUUIDString(nwfilter, uuid);
        newUuid = virtDBusUtilEncodeUUID(uuid);
        return g_strdup_printf("%s/%s", nwfilterPath, newUuid);
    }

    void
    virtDBusUtilVirNWFilterListFree(virNWFilterPtr *nwfilters)
    {
        for (gint i = 0; nwfilters[i] != NULL; i++)
            virNWFilterFree(nwfilters[i]);

        g_free(nwfilters);
    }
    ```
