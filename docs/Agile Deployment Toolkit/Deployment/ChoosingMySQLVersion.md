#### SELECTING MYSQL VERSION

If you are using a self managed version of MySQL (most likely if you are developing an application at least) then you can set which version of MySQL you intend to use. If you are using a managed solution the technique for setting which version you will use is different. You might not have any choice the managed solution might, for example simply say "MySQL 8"  

Anyway for the self managed MySQL option you can set the version in  

>     ${HOME}/configuration/software.dat

The line of the file will be something like

>     MYSQL:repo:9.2.0

And this will install version 9.2.0 of MySQL.

I mention MySQL specifically because there is something you need to be aware of before you set which version that you want to deploy and that is that you need to know that a installable archive is available for the OS version and MySQL version you are choosing.

To do this you need to go to [this](https://downloads.mysql.com/archives/community) URL.  

So, for example, if I want to install mysql version 9.3.0 on debian 12 then I need to see an archive on the website with the nomenclature:

mysql-server_**9.3.0**-1debian**12**_amd64.deb-bundle.tar  

If I don't see one then the build will fail. If I want to install mysql 9.5.0 on debian 13 then I need to see:

mysql-server_**9.5.0**-1debian**13**_amd64.deb-bundle.tar   

Similarly mysql 9.3.0 on ubuntu 24.04 will mean I need to see

mysql-server_**9.3.0**-1ubuntu**24.04**_amd64.deb-bundle.tar  

on the mysql site. 

So, you need to match the version of MySQL and the version of the OS you are deploying with the MySQL archive I have pointed you towards and if those versions match the archive then the deployment should work OK. 
