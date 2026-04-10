**/var/www/html/dba.dat**

This file is written to the webroot of every baselined application and it contains the name of the application type that this webroot is. 

If its a joomla application, this file contains JOOMLA  
If its a wordpress application, this file contains WORDPRESS  
If its a drupal application, this file contains DRUPAL  
If its a moodle application, this file contains MOODLE  

This file being present means we can quickly check which application type this application webroot is

-------

**/var/www/html/dbe.dat**

There is a file in the webroot of baselined applications which tell you which database it was built with.  

This file is 

>     /var/www/html/dbe.dat  

and it will tell you  whether the application was baselined with MariaDB, MySQL or Postgres  

You should only deploy using the same database as is written to this file in other words, MariaDB and MySQL are not considered 100% interchangeable although they are supposed to be drop in replacements. 
Also, if you intend to deploy to a managed database that runs MARIADB, for example, you should do your application development against a MARIADB instance you shouldn't develop against MySQL if your ultimate target is a MARIADB instance managed database. 
To be clear, you could probably get away with using MySQL and MariaDB interchangeably, but, caution and wanting to avoid issues makes me say try not to if at all possible. 

---------

**/var/www/html/dbp.dat**

The database prefix is written to this file so we can easily see which database prefix was set when this application was baselined which means we expect the database to have tables prefixed with the same prefix

-------------

**/var/www/html/dbt.dat**

This is necessary for drupal based application deployments and can be set to:

- "SOCIAL_DRUPAL" - if you are deploying Opensocial
- "CMS_DRUPAL" - if you are deploying Drupal CMS
- "DRUPAL" - If you are deploying regular Drupal

