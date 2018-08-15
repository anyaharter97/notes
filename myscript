#!/usr/bin/env bash
#
# Developed by Anya Harter <aharter@redhat.com> on 2018-08-03
# With help from Kevin Postlethwait <kpostlet@redhat.com>
#
# Last Updated 2018-08-15
#
# Pass in the name of the test suite to run
#

tabs 11 17 57 65 75 85 95 105

today=`date '+%Y%m%d_%H%M%S'`
filename="$PWD/Test-cockpit-$1-results_$today.log"

if [ $1 = "run-tests" ]
then
  echo "Running all test suites..."
else
  echo "Starting test suite '$1'"
fi

echo "-----------------------------------------------------------------"
printf " \033[1;37mSTATUS\t #\tNAME\tDURATION\033[1;000m\n"
echo "-----------------------------------------------------------------"

./test/verify/$@ 2>&1 | tee $filename \
| gawk '{gsub(/not ok/,"\033[0;31m FAILURE  \033[1;000m")}{gsub(/\<ok\>/,"\033[0;32m SUCCESS  \033[1;000m")}{gsub(/__main__/,"")}{gsub("\\.","")}{gsub("[0-9]+ ","&\t")}{gsub("# duration: ","\t")}/FAILURE|SUCCESS/'

testsfailed=`gawk '{gsub("#","\033[7;31m")}{gsub("]","&\033[1;000m")}/FAILED/' $filename`
testspassed=`gawk '{gsub("#","\033[7;32m")}{gsub("]","&\033[1;000m")}/PASSED/' $filename`

if ! [ -z "$testsfailed" ]
then
    echo "-----------------------------------------------------------------"
    echo "$testsfailed"
    echo "-----------------------------------------------------------------"
elif ! [ -z "$testspassed" ]
then
    echo "-----------------------------------------------------------------"
    echo "$testspassed"
    echo "-----------------------------------------------------------------"
else
    echo "-----------------------------------------------------------------"
    echo -e " \033[7;33mTEST SUITE FAILED TO TERMINATE\033[1;000m"
    echo "-----------------------------------------------------------------"
fi

tabs 8
