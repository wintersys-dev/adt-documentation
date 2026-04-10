In the 

>     ${HOME}/configuration/software.dat

file you can set the version of MYSQL if you are performing a self hosted solution rather than a managed solution.  
The line of the file will be something like

>     MYSQL:repo:9.2.0

And this will install version 9.2.0 of mysql

There's some things you need to take note of before beginning a build which are that you need to know that a installable archive is available
for the OS version and MySQL version you are choosing.

To do this you need to go to [this](https://downloads.mysql.com/archives/community) URL. 
So, for example, if I want to install mysql version 9.3.0 on debian 12 then I need to see an archive on the website with the nomenclature:

mysql-server_**9.3.0**-1debian**12**_amd64.deb-bundle.tar  

If I don't see one then the build will fail. If I want to install mysql 9.5.0 on debian 13 then I need to see:

mysql-server_**9.5.0**-1debian**13**_amd64.deb-bundle.tar   

Similarly mysql 9.3.0 on ubuntu 24.04 will mean I need to see

mysql-server_**9.3.0**-1ubuntu**24.04**_amd64.deb-bundle.tar  

on the mysql site
