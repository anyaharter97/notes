# Cockpit Machines
* [Testing](cockpit.md#testing)

Cockpit is a web-based linux host management interface  

Headless systems are VMs with no graphical interface  

*Cockpit-Machines* is the virt-plugin for Cockpit, the simple use case is VM management

### Installing on your Local Machine
1. Follow the instructions specific to your operating system at https://cockpit-project.org/running.html to install and set up cockpit

2.

`systemctl start cockpit`

`~/git/cockpit/HACKING.md`


clone git repo
`mkdir build`
`cd build`
`npm install .`
`../autogen.sh --prefix=/usr --enable-debug`

`mkdir -p ~/.local/share/cockpit`
`ln -sT /home/aharter/git/cockpit/dist/machines machines` <= inside local share
`npm install -g webpack@1.13.2`
`webpack --progress --watch --color`


`git clean -fdx`
`mkdir build`
`cd build`
`../autogen.sh --prefix=/usr --enable-debug`


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

    * Alternatively, use my **~AWESOME~** script ([`/usr/bin/myscript`](myscript)) by calling it as follows:

    ```
    $  source myscript check-machines
    ```

        * An additional **~AWESOME~** script takes the test output and prints the same format output ([`/usr/bin/myscript2`](myscript2))

4. To test JavaScript syntax run the following command from the top level of the cockpit directory, which prints only 2 lines on success

    ```
    $  npm run eslint
    ```
