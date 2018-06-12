# Command Line

## Configuration
`~/.bashrc` contains most of the custom configuration of the terminal  
`~/.bash_profile` contains more configuration but only gets invoked upon login, so this is where you put more time intensive configuration that you don't want to happen every time you open a command line  

## Permissions

Unix file permissions consist of tags:
* `-r`: read
* `-w`: write
* `-x`: executable

These permissions can be changed using the `chmod` command.  
File ownership can be changed using the `chown` command.  

#### Running as Root / With Privileges
Use the keyword `sudo` before any command to run from root.  
You can also use the command:
  ```
  $  sudo -
  $  # do a bunch of admin commands
  $  exit
  ```
If you are going to need to run a lot of restricted commands in a row.

## Installing Software

#### Gnome
Gnome Software is essentially the app store for Linux.

#### Installing
Use yum to install anything that is already in the repo.  
&nbsp;&nbsp;&nbsp;&nbsp;\* You will need to use `sudo` to yum install things, for example:
  ```
  $  sudo yum install <thing-to-install>
  ```

## The `Less` Command
Pipe commands to `less` to view things in a page-able mode. This makes things easier to read and navigate. Particularly with large chunks of text such as git diffs etc.
An example command is
  ```
  $  sudo yum list --installed | less
  ```
to display all of the programs installed.

The `less` command also puts you into a sort of vim-like mode where you can search using vim hotkeys etc.

## Package Files Commands
* `rpm -ql NetworkManager` lists all of the files that are in the package NetworkManager  
* `which nmcli` tells you where it is  
* ``rpm -qf `which nmcli` `` tells you which package owns it  

## Directories
* `/etc` contains all system related configuration files in here or in its sub-directories, this is where NetworkManager lives  
* `/usr` has a lot of good stuff in it  
  * `/usr/bin` and `/usr/sbin` contain mainly data and executables installed by packages (both are always in `$PATH`)  
  * `/usr/include` has a bunch of header files that can be used in includes in code  
  * `/usr/share` has mostly data and documentation  
* `/home` is what is more accurately "user" stuff  
* `/tmp` is like a standard temp except that it has an auto-retention policy  
* `/root` is root's home directory  

#### Navigation
* `cd` and `cd ~` take you home  
  * `cd -` takes you back to where you were   
  * `cd ..` takes you up one directory (you can move multiple levels at a time with `cd ../../`)  
* `pwd` is print working directory  
* `ls` shows all files in cwd (current working directory)  
  * `ls -l` shows details  
  * `ls -a` shows all files (including hidden files)  

## Miscellaneous Commands
* `git grep` helps you find things  
* `ctr` + `d` will close the terminal  
* `find . -name \*~` will find all files ending in ~ (the backslash escapes the \*)  
* `ps axww` displays details about processes
