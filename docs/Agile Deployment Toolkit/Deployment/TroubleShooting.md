If a build doesn't complete or is "frozen" and you aren't sure why every .sh file on the build-machine has a function

>     status () {
>             /bin/echo "${1}" | /usr/bin/tee /dev/fd/3 2>/dev/null
>             script_name="`/bin/echo ${0} | /usr/bin/awk -F'/' '{print $NF}'`"
>             /bin/echo "${script_name}: ${1}" >> /dev/fd/4
>     }

This function will write to 

>     ${BUILD_HOME}/runtimedata/linode/crew/logs/build-status-stream-${date-and-time}

and so you can call this function from any of the .sh files on the build-machine by adding a status command to the part of the file that you suspect with any debug info that you need by calling

>     status "Got as far as here with cloudhost set to ${cloudhost}"

for example and that will give you a trace complete with the file name that is being accessed. The output to stdout doesn't contain the filename and its useful to have a record stored on the filesystem of a build attempt as well.


