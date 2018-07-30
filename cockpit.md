# Cockpit Machines
* [Testing](cockpit.md#testing)

Cockpit is a web-based linux host management interface  

Headless systems are VMs with no graphical interface  

*Cockpit-Machines* is the virt-plugin for Cockpit, the simple use case is VM management

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

    * Alternatively, use my **~AWESOME~** script

    `~/tmp/myscript`:

    ``` bash
    #!/usr/bin/env bash

    echo "Starting test suite"

    ~/git/cockpit/test/verify/check-machines &> $PWD/Test-cockpit-check-machines-results.log

    echo "Finished test suite"

    sed -rn "s/^((not )?ok) ([0-9]*) (.*) \(__main__.(.*)\) (.*)/\1\t\3\t\5.\4\t\6/p" "$PWD/Test-cockpit-check-machines-results.log" | column -t -s $'\t'
    ```

    by calling it as follows:

    ```
    $  source ~/tmp/myscript
    ```
