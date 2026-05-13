#### DATABASE INITIALISATION AND CONFIGURATION SCRIPTS

The databases need to be initialised and configured before they are used. They need to be initialised with usernames and privileges as well as have their configuration values set according to the use case. These initialisation scripts and configuration scripts are held in the files located under the relevent class of database. And so here I simply list the locations of the different files which are config or initialisation files. You will notice that there is a "live" direcory and an "inactive" directory. This means that you can keep different configurations "inactive" and copy them into and out of the "live" directory depending on which configuration or initilisation type you want to have "live" or "active". As soon as you copy a configuration or intialisation file into the "live" directory (as long as it has the correct name as shown here) that file will become the "active" or "live" initialisation or configuration file for the next deployment of the corresponding database.


>     ${HOME}/services/database/selfmanaged/postgres/live:  
>     total 4  
>     -rw-r--r-- 1 root root 227 May 21 22:19 postgres.psql ( you can write any intialisation you want for your datbase here in psql )  

>     ${HOME}/services/database/selfmanaged/postgres/inactive: (you can store currently inactive psql scripts here and copy them to 'live' when you want to use them)  
>     total 8  
>     -rw-r--r-- 1 root root 283 May 21 22:19 with-superuser.psql  
>     -rw-r--r-- 1 root root 227 May 21 22:19 without-superuser.psql  

>     ${HOME}/services/database/selfmanaged/mysql/live:  
>     total 8  
>     -rw-r--r-- 1 root root 1071 May 21 22:19 mysql.sql  ( you can write any intialisation you want for your datbase here in sql )  
>     -rw-r--r-- 1 root root  731 May 21 22:19 mysql.config (you can configure mysql anyway you want using this file )  

>     ${HOME}/services/database/selfmanaged/mysql/inactive:  
>     total 12  
>     -rw-r--r-- 1 root root 1160 May 21 22:19 root-enabled.sql   (you can store currently inactive mysql scripts here and copy them to 'live' when you want to use them)  
>     -rw-r--r-- 1 root root 1071 May 21 22:19 root-disabled.sql  
>     -rw-r--r-- 1 root root  731 May 21 22:19 mysql.config.default  

>     ${HOME}/services/database/selfmanaged/mariadb/live:  
>     total 8   
>     -rw-r--r-- 1 root root 1078 May 21 22:19 mariadb.sql     ( you can write any intialisation you want for your datbase here in sql )  
>     -rw-r--r-- 1 root root  770 May 21 22:19 mariadb.config  (you can configure mariadb anyway you want using this file )  

>     ${HOME}/services/database/selfmanaged/mariadb/inactive:  
>     total 12  
>     -rw-r--r-- 1 root root 1139 May 21 22:19 root-enabled.sql (you can store currently inactive mariadb scripts here and copy them to 'live' when you want to use them)  
>     -rw-r--r-- 1 root root 1077 May 21 22:19 root-disabled.sql  
>     -rw-r--r-- 1 root root  701 May 21 22:19 mariadb.config.default  
