# Cockpit Machines
* [Installing on your Local Machine](cockpit.md#installing-on-your-local-machine)
* [Testing](cockpit.md#testing)

Cockpit is a web-based linux host management interface  

Headless systems are VMs with no graphical interface  

*Cockpit-Machines* is the virt-plugin for Cockpit, the simple use case is VM management

### Installing on your Local Machine
More details in `~/git/cockpit/HACKING.md` but this outlines how to set up Cockpit to run just the machines package from the git repo.
1. Follow the instructions specific to your operating system at https://cockpit-project.org/running.html to install and set up cockpit

2. Start the cockpit service using the command

    ```
    $ systemctl start cockpit
    ```

3. Create a fork of the git repo and then clone it onto your system

4. Create a build directory in the top level of the git repo to keep your build separate from your system and build the project there using the following steps

    ```
    $  mkdir build
    $  cd build
    $  npm install .
    $  ../autogen.sh --prefix=/usr --enable-debug
    ```

    Don't ever run `make install` because it will mix in the cockpit from git and the one already on the system and makes everything super complicated.

5. Create a link from the Cockpit Machines package in the distribution folder using the following steps

    ```
    $  mkdir -p ~/.local/share/cockpit
    $  cd ~/.local/share/cockpit
    $  ln -sT /aboslutepathtocockpit/dist/machines machines
    ```

6. Set up webpack to build Cockpit from the source files by running the following commands from the top of the git repo

    ```
    $  npm install -g webpack@1.13.2
    $  webpack --progress --watch --color
    ```

    From now on, you should only have to run the second command to build the project. The --watch flag watches your files for changes and updates the browser accordingly. Without this flag, you would have to rerun the webpack command every time a change was made. Never run `git rebase master` with webpack running in watch mode because it can crash the program.

7. If you run into problems, you can try the following steps to clean and rebuild the project

    ```
    $  git clean -fdx
    $  mkdir build
    $  cd build
    $  ../autogen.sh --prefix=/usr --enable-debug
    ```

### Testing
More details in `/test/README`

1. Download the test images using the following command with your OS

    ```
    $  ./bots/image-download fedora-28
    ```

2. Prepare the test images similarly

    ```
    $  ./bots/image-prepare fedora-28
    ```

3. Run the test suite

    * To run the full test suite use command

        ```
        $  ./tests/verify/run-tests
        ```

    * To run the test suite for Cockpit Machines use command

        ```
        $  ./tests/verify/check-machines
        ```

    * Alternatively, use my **\~AWESOME\~** script ([`/usr/bin/myscript`](myscript)) by calling it as follows:

        ```
        $  source myscript check-machines
        ```

        * An additional **\~AWESOME\~** script takes the test output and prints the same format output ([`/usr/bin/myscript2`](myscript2))

4. To test JavaScript syntax run the following command from the top level of the cockpit directory, which prints only 2 lines on success

    ```
    $  npm run eslint
    ```
