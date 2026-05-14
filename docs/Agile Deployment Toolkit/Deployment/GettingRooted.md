#### GETTING ROOTED

When you access your servers using the helper scripts from your build machine or laptop, you are accessing them with a limited capacity user. You need to become the root user to have full power over your servers so it is presumed that your limited capacity user (agile-deployer) is a secured account and anyone with access to it has been properly authenticated. And so there is a helper script called "Super.sh" which you can run without providing passwords interactively which will get you to "root" in order to make the process as frictionless as possible.

You will need root access to your autoscaler(s), webserver(s) and database machine.

To do this, on your build machine, go into the "helpers/servers" directory of your ADT and use the "ConnectToXXXX" scripts to connect to your servers as required.
Once you are on your server issue the following commands (because you must have the private key to access the servers the servers trust you) and so,

>     cd ${HOME}/super   
>     /bin/sh Super.sh 

This will make you root on that machine and its full steam ahead from there
