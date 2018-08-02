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

    `/usr/bin/myscript`:

    ``` bash
    #!/usr/bin/env bash

    tabs 11 17 57

    echo "Starting test suite '$1'"

    echo "-----------------------------------------------------------------"
    printf " STATUS\t #\tNAME\tDURATION\n"
    echo "-----------------------------------------------------------------"

    today=`date '+%Y%m%d_%H%M%S'`
    filename="$PWD/Test-cockpit-$1-results_$today.log"

    ~/git/cockpit/test/verify/$1 2>&1 | tee $filename \
    | gawk '{gsub(/not ok/,"\033[0;31m FAILURE  \033[1;000m")}{gsub(/\<ok\>/,"\033[0;32m SUCCESS  \033[1;000m")}{gsub(/__main__/,"")}{gsub("\\.","")}{gsub("[0-9]+ ","&\t")}{gsub("# duration: ","\t")}/FAILURE|SUCCESS/'


    echo "-----------------------------------------------------------------"
    gawk '{gsub("#","\033[0;33m")}{gsub("]","&\033[1;000m")}/FAILED/' $filename
    echo "-----------------------------------------------------------------"

    tabs 8
    ```

    by calling it as follows:

    ```
    $  source myscript check-machines
    ```

4. To test JavaScript syntax run the following command from the top level of the cockpit directory, which prints only 2 lines on success

    ```
    $  npm run eslint
    ```
