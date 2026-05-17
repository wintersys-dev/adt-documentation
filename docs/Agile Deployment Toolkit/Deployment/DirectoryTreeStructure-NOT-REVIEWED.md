Here you will find an expose on the directory structures of the various different machine types with a brief explanation of what you will find. 

#### On the build machine

```${BUILD_HOME}/descriptors/autoscaler_descriptor.dat``` - the autoscaler relevant environment variables  
```${BUILD_HOME}/descriptors/database_descriptor.dat``` - the database relevant environment variables  
```${BUILD_HOME}/descriptors/webserver_descriptor.dat``` - the webserver relevant environment variables   
```${BUILD_HOME}/descriptors/reverseproxy_descriptor.dat``` - the reverseproxy relevant environment variables  
```${BUILD_HOME}/descriptors/authenticator_descriptor.dat``` - the authenticator relevant environment variables   

```${BUILD_HOME}/configuration/software.dat``` - configuration of different build methods for the tools we want to work with  
```${BUILD_HOME}/configuration/firewall.dat``` - firewall settings of the different server machines  

```${BUILD_HOME}/runtime```  
This is the directory where general (build-independent) configuration details and files are generated as part of the build process. 

```ACTIVE_BUILD_IDENTIFIER``` - This is the currently active build_identifier which gets refreshed each time a new build cycle begins
```ACTIVE_CLOUDHOST``` - This is the currently active cloudhost which gets refreshed each time a new build cycle begins
```BUILD_MACHINE_CLOUDHOST``` - This is the currenlty active build machine cloudhost (which can be different to the ACTIVE_CLOUDHOST which is the cloudhost that your main servers are running on
```BUILDMACHINEPORT:<port>``` - This is the build machine SSH port in other words, this is the SSH port that you can connect to your build machine through
```EXUPDATEDSOFTWARE``` - This is a flag that tells us whether the core software has been updated or not
```LAPTOPIP:<laptop-ip>``` - This tells us the laptop ip address for the laptop machine that originated the build

```${BUILD_HOME}/runtime/<cloudhost>/<build-identifier>```

This is the dirctory where build specific configuration details and configuration files are generated as part of the build process

```application``` - This directory holds the application specific descriptor which contains the configuration details necessary to install the application  
```APPLICATION:<application_name>``` - This tells us which application is currently being installed for this build cycle  
```autoscaler_configuration_settings.dat``` - This contains all the configuration settings necessary to build an autoscaler class of machine  
```build_environment``` - This is a file containing all the environment variables that were set in the template  
```cloud-init``` - This directory contains all the cloud-init scripts that will be passed to the servers when they are provisioned  
```credentials``` - Here are the credentials for the servers  
```CURRENTREGION``` - This file contains the current region for this build cycle  
```datastore_workarea``` - This is a work area for the datastore manipulation scripts to use during their functional operation  
```dbp.dat``` - This is the database prefix for our application  
```EMERGENCY_PASSWORD``` - This is the password for our servers that can be used through GUI based console connection if there is some deep problem with a server  
```ips``` - This directory can contain IP addresses of machines as needed for the correct functioning of our processes  
```keys``` - This directory contains the SSH keys that are needed by ${BUILD_HOME}/helpers/servers to connect to different classes of machine  
```logs``` - Here the log files that inform us about how the build process is going are stored and can be referred to when trouble shooting  
```ssl``` - This directory contains the ssl certificates for the current build cycle. This certificates will be long lasting if your webserver uptime is long lasting  
```TOKEN``` - This is a file for storing the access token for our cloud provider CLI  
```VPC-ACTIVE``` - If this file exists then it tells us that the servers are running inside a VPC  
```autoscaler_configuration_settings.dat``` - this contains all the configuration settings necessary to build a autoscaler class of machine  
```authenticator_configuration_settings.dat``` - this contains all the configuration settings necessary to build a authenticator class of machine  
```reverseproxy_configuration_settings.dat``` - this contains all the configuration settings necessary to build a reverseproxy class of machine  
```database_configuration_settings.dat``` - this contains all the configuration settings necessary to build a database class of machine  
```webserver_configuration_settings.dat``` - this contains all the configuration settings necessary to build a webserver class of machine  

```${BUILD_HOME}/buildservers``` This directory is here for holding scripts which do the main builds of different machine types in the build chain such as autoscaler type machines, webserver type machines and database time machines. Other machine types can be added to build chains such as Caching machine types if you wanted to extend to the toolkit to provide support for caching systems as well.

```${BUILD_HOME}/helpers``` - Here you will find helpers which can do things like performance of interactive machine backups or connecting to different machine types over ssh with the management of the requisite keys managed by the scripts for you

```${BUILD_HOME}/initialisation``` - This directory is for scripts which perform various initialisation processes such as the initiation of error reporting or a datastore initiation 

```${BUILD_HOME}/installation``` - I use just regular apt mostly to install the software and the idea is to have installation scripts which can be written and added for any additional software that you want to install in the future or if you want to install a particular software using a different installation method. And so all install scripts should be located here for organisational reasons and convenient access as well.

```${BUILD_HOME}/migration``` - If you are migrating the code base of, say, a joomla application from a different hosting provider this directory will likely be relevant to you according the recommended migration process which you can find elsewhere in this documentation

```${BUILD_HOME}/processing``` - The scripts located here are for doing any kind of application specific processing that is required. Its conceivable that during a build some application types might need speical treatment which you can write code for and apply here if you need to.

```${BUILD_HOME}/selection``` - This is basically for interactive scripts where a partcular selection needs to be made, for example, which cloudhost you are deploying to and so on.

```${BUILD_HOME}/templatedconfiguration``` - Anything to do with the templates that are used to perform the build prcess is located here. In ordinary operation you will most likely clone the ADT and head to this directory to populate the variables of the appropriate template for your cloudhost of choice and build style with the values necessary for the build to proceed. 

----------------------------

#### Autoscaler  machines

```${HOME}/runtime/beingbuiltips``` - this directory stores the ip addresses of the machines that are currently considered to be in the process of being built  

```${HOME}/runtime/beingbuiltpublicips```  - this directory stores the public ip addresses of the machines that are currently considered to be in the process of being built  

```${HOME}/runtime/POTENTIAL_STALLED_BUILD:${private_ip}``` - A webserver build process is considered potentially stalled until the build completes. If this marker file is still present after a protracted period of time then the build is consider actually stalled. When the build for a webserver with a given IP address completes, this file is removed.   

```${HOME}/runtime/AUTOSCALINGMONITOR:${1}``` - Webservers are built in batches. Once a batch of webservers have been actioned to be built, the scaling mechanism to action additional builds is blocked until all the webserver builds in the batch are either considered stalled or complete  

```${HOME}/runtime/INITIALLY_PROVISIONING-${buildno}.lock``` - when a webserver is actioned to be built, the initial machine provisioning is considered to be the "INITIALLY_PROVISIONING" state and it only once a new webserver machine is pingable that it is considered to have completed its INITIAL_PROVISIONING  

```${HOME}/runtime/probed_ips``` - In the process of checking whether webservers are online or not a list of probed IPs are kept and these probed IP addresses or the IP addresses that have been checked for their online status during the current cycle of checking are stored here.   

```${HOME}/runtime/potentialenders``` - When we are monitoing for webservers that need to be shutdown and destroyed, we keep a list of those webservers that we think potentially need to be ended in this directory  

```${HOME}/runtime/software.dat``` - this file is a configuration file obtained from the build machine which gives us the details of how software packages need to be installed on this autoscaler machine (for example are they to be built from sourcecode or repositories)  

```${HOME}/runtime/firewall.dat``` - This is a configuration file obtained from the build machine which relates to how the firewall is configured  

```${HOME}/runtime/application.dat``` - This is a configuration file obtained from the build machine which holds what and how an application is being installed for this deployment  

```${HOME}/runtime/cloud-init``` - this directory contains the cloud-init scipts which will be used to initialise each webserver  

```${HOME}/runtime/NOT_AUTHORISED_TO_SCALE``` - This marker file will be present when the current autoscaler is authorised to scale  

```${HOME}/runtime/AUTHORISED_TO_SCALE``` - This marker file will be present when the current autoscaler is authorised to scale  

```${HOME}/runtime/filesystem_sync``` - this directory relates to the system process that will synchronise filesystems between machines when and as actioned from cron  

```${HOME}/runtime/datastore_workarea``` - this is a workarea related to datastore functions. The scripts which interact with the S3 datastore are welcome to write what they need to into this area as (like the name says), a workarea.   

```${HOME}/runtime/dbaas_allowed_ips``` -  When a DBaaS solution is being used in multi-region mode, then, this is the list of IP addresses of the webservers that are allowed to connect to the DBaaS instance. The public ip address of a webserver being run by a different vendor to where the DBaaS instance is running must have its public IP address in the list of allowed IP addresses of the DBaaS instance for it to be able to connect 

```${HOME}/runtime/zoneid.dat``` - Cloudflare needs to know what the current zone is for its DNS interactions so this is where that information is stored.   

```${HOME}/runtime/SSMTP_INITIALISED``` - This marker file is present when it is considered that the SMTP system has been intialised (in other words, system emails can be actioned and sent)  

```${HOME}/runtime/KNICKERS_ARE_UP``` - This maker file indicates that the incoming connections to our server have been initially blocked and that incoming connections will then be allowed on a discretionary basis  

```${HOME}/runtime/FIREWALL-ACTIVE``` - This marker file tells us that the firewall is currently active  

```${HOME}/runtime/FIREWALL-INITIALISED``` - This marker file tells us that the firewalling system has been initialised  

```${HOME}/runtime/autoscaler_configuration_settings.dat``` - This is the configuration file obtained from the build machine for this current autoscaler  

```${HOME}/runtime/webserver_configuration_settings.dat``` - This is the configuration file obtained from the build machine that will be utilised for the building and provisioning of any webservers by this autoscaler  

```${HOME}/runtime/scaling``` - This directory will hold a record of how many webservers are expected to be provisioned by this autoscaler  

```${HOME}/runtime/ssh-audit``` - we can look here to see what the ssh interactions have been for this current machine  

```${HOME}/runtime/virus_report``` - If we want to scan for viruses (which can protect windows users in extremely rare cases, the report will be here)  

```${HOME}/runtime/installedsoftware``` - This directory contains a list of bespoke marker files which tell us what software has been installed on our current machine  

```${HOME}/runtime/CPU_OVERLOAD_ACKNOWLEDGED``` - this marker file tell us that a notifcation has been made because of CPU Overload on the current machine  

```${HOME}/runtime/LOW_MEMORY_ACKNOWLEDGED``` - this marker file tell us that a notifcation has been made because of Low Memory on the current machine  

```${HOME}/runtime/LOW_DISK_ACKNOWLEDGED```  - this marker file tell us that a notifcation has been made because of Low Disk space on the current machine

```${HOME}/autoscaler``` - This directory contains the scripts which provide the autoscaling mechanism and functionality

```${HOME}/cron``` - This directory contains the scripts relating to cron functionality

```${HOME}/installation``` - This directory contains all the scripts which relate to installation of software and so on

```${HOME}/system``` - This is where all scripts related to third party providers are kept such as datastore providers, git providers and so on

```${HOME}/security``` - This directory has scripts that relate to security such as the firewall  

----------------------------

#### Webserver machines

```${HOME}/runtime/AUTOSCALED_WEBSERVER_ONLINE``` - this flag is set from an autoscler on the current webserver to let us know that this webserver is online and was generated as a scaling event rather than as part of initial infrastructure provisioning  

```${HOME}/runtime/APPLICATION_DB_GENERATED``` - this flag can be used when an application needs to install its database as part of the build process rather than through interaction with the user. This flag is set when the database is successfully installed. Moodle uses this flag to tell us when the initial database has been installed

```${HOME}/runtime/AUTOSCALED_WEBSERVER_ONLINE``` - this can be set if the current webserver is provisioned through an autoscaling event and is considered to be online and primed

```${HOME}/runtime/BUILDCLIENTIP``` - this is a convenient place to hold the ip address of the build machine for easy access

```${HOME}/runtime/CACHE_CLEANED``` - if we want to clean our application cache at as the application installs this flag lets us know that the cache has been cleaned

```${HOME}/runtime/DATASTORE_CACHE_PURGED``` - this is a flag that tells us when a datastore mounted filesystem has had its cache purged. We can test for this flag and purge the cache based on what we find. 

```${HOME}/runtime/CPU_OVERLOAD_ACKNOWLEDGED, ${HOME}/runtime/LOW_DISK_ACKNOWLEDGED, ${HOME}/runtime/LOW_MEMORY_ACKNOWLEDGED``` - this flag tells us whenever we have notified the user by email of some sort of low resource situation. Using this flag it means we only send emails periodically rather than multi times consequtively in short order as we might do without these flags  

```${HOME}/runtime/CREDENTIALS_PRIMED``` - once we have got the database credentials that were generated on the build machine on our current webserver we consider credentials to be primed (in other words, we know what our database credentials are if this is set).

```${HOME}/runtime/INITIAL_CONFIG_SET``` - this is set when the application's initial configuration has been set. This stops it being set again unless there is a difference that needs to be taken into account

```${HOME}/runtime/drupal_settings.php``` - the drupal configuration file
```${HOME}/runtime/wordpress_config.php``` - the wordpress configuration file
```${HOME}/runtime/joomla_configuration.php``` - the joomla configuration file
```${HOME}/runtime/moodle_config.php``` - the moodle configuration file

```${HOME}/runtime/FIREWALL-ACTIVE``` - this flag will be set if we consider the firewall active

```${HOME}/runtime/GARBAGE_CLEANED``` - if an application needs any cleaning up done during its install, this flag will have been set once the garbage is cleaned

```${HOME}/runtime/installedsoftware``` - this directory serves as a record as to which software has been installed on this machine. It can be referred to if the software needs to be updated so that we know what packages to update and what pacakges to leave alone

```${HOME}/runtime/KNICKERS_ARE_UP``` - this is to do with the firewall it means that we have set our base condition which is to allow outgoing connections but dissalow all incoming connections and so basically, "knickers are up" because no one is let in.

```${HOME}/runtime/MARKEDFORSHUTDOWN``` - for ease we can mark a machine for shutdown and the shutdown will then be actioned

```${HOME}/runtime/PREPARE_MOUNTS``` - this means that the s3 mounts are prepared for datastore assets mounted to the current webroot

```${HOME}/runtime/SETTING_UP_ASSETS``` - this is present when datastore mount assets are being prepared. This means that you won't get two attempts to mount if a mount is already in process for some bizarre reason

```${HOME}/runtime/sslcertlock.file``` - this is a lock file for generating SSL certificates which must not be present if a new attempt to generate an SSL certificate can proceed. 

```${HOME}/runtime/updated_webroot.dat``` - if there are any new files in the webroot their paths are stored here and then they are copied to the datastore for distribution to the other webroots

```${HOME}/runtime/WEBSERVER_READY``` - this is set if the webserver has completed its initial build

```${HOME}/cron``` - This directory contains the scripts relating to cron functionality

```${HOME}/installation``` - This directory contains all the scripts which relate to installation of software and so on

```${HOME}/system``` - This is where all scripts related to third party providers are kept such as datastore providers, git providers and so on

```${HOME}/security``` - This directory has scripts that relate to security such as the firewall  

```${HOME}/runtime/webserver_configuration_settings.dat ${HOME}/runtime/software.dat``` - these files are the configuration settings for our current webserver  

```${HOME}/runtime/ALL_CORE_SOFTWARE_INSTALLED``` - this is just a placeholder which tells us that the core software has all been installed

```${HOME}/runtime/WEBSERVER_CONFIG_LOCATION.dat``` - this stores the location of the webserver configuration file for ease of access elsewhere

------------------------------

#### Database Machines

```${HOME}/runtime/DB_APPLICATION_INSTALLED``` - If this file is present then it means that the system considers an application's data to have been installed into the database from an SQL dump file

```${HOME}/runtime/BUILDCLIENTIP``` - this is a placeholder for the build machine's ip address. It is stored here for convenience.  

```${HOME}/runtime/CPU_OVERLOAD_ACKNOWLEDGED, ${HOME}/runtime/LOW_DISK_ACKNOWLEDGED, ${HOME}/runtime/LOW_MEMORY_ACKNOWLEDGED``` - this flag tells us whenever we have notified the user by email of some sort of low resource situation. Using this flag it means we only send emails periodically rather than multi times consequtively in short order as we might do without these flags  

```${HOME}/runtime/CREDENTIALS_PRIMED``` - once we have got the database credentials that were generated on the build machine on our current webserver we consider credentials to be primed (in other words, we know what our database credentials are if this is set).

```${HOME}/runtime/DATABASE_APPLICATION_UPDATING``` - this is set when the application database is being updated if a database machine is being provisioned using a snapshot

```${HOME}/runtime/DATABASE_READY``` - if the database has completed its build, this file will be present

```${HOME}/runtime/dbinstalllock.file``` - if prsent this means that the database is currenlty being installed. You can't install the actual database if this lock file is present

```${HOME}/runtime/FIREWALL-ACTIVE``` - this file is present if we believe that the firewall is active


```${HOME}/runtime/mariadb-init/initialiseDB.sql``` - this is the intialisation configuration that creates the application database and username/password in your mariadb database

```${HOME}/runtime/mysql-init/initialiseDB.sql``` - this is the intialisation configuration that creates the application database and username/password in your mysql database

```${HOME}/runtime/postgres-init/initialiseDB.sql``` - this is the intialisation configuration that creates the application database and username/password in your postgres database

```${HOME}/runtime/installedsoftware``` - this directory serves as a record as to which software has been installed on this machine. It can be referred to if the software needs to be updated so that we know what packages to update and what pacakges to leave alone

```${HOME}/runtime/KNICKERS_ARE_UP``` - this is to do with the firewall it means that we have set our base condition which is to allow outgoing connections but dissalow all incoming connections and so basically, "knickers are up" because no one is let in.

```${HOME}/applicationdb``` - the files to do with the specific application we are installing

```${HOME}/cron``` - This directory contains the scripts relating to cron functionality

```${HOME}/installation``` - This directory contains all the scripts which relate to installation of software and so on

```${HOME}/system``` - This is where all scripts related to third party providers are kept such as datastore providers, git providers and so on

```${HOME}/security``` - This directory has scripts that relate to security such as the firewall  

```${HOME}/runtime/database_configuration_settings.dat ${HOME}/runtime/software.dat``` - these files are the configuration settings for our current database machine

---------------------------------

#### Datastore structure and function:

```STATIC_SCALE:[no]``` - this tells us what the current number of webservers should be. If this number is changed then there will be a scale up or scale down of the number of webservers running  

```autoscalerip/``` - this directory contains the private IP addresses of the currently active autoscaler machines  

```autoscalerpublicip/``` - this directory contains the public IP addresses of the currenlty active autoscaler machines  

```webserverpublicips/``` - this directory contains the private IP addresses of the currently active webserver machines  

```webserverips/``` - this directory contains the public IP addresses of the currently active webserver machines  

```databaseip/``` - this directory contains the private IP addresses of the currently active database machine  

```databasepublicip/``` - this directory contains the public IP addresses of the currently active database machine  

```buildclientip/``` - this directory contains the ip address of the build machine   

```overloadedips/``` - any ip addresses that are considered overloaded are stored here   

```beenonline/``` - if a machine has ever been online its IP address is written here this helps us distinguish between machines which might have IP addresses available but have never been fully online yet  

```INSTALLED_SUCCESSFULLY``` - this is a flag which we can check which tells us that the entire build process has been considered to have completed successfully.   

```webroot-update/``` - a holding area that we sync updated webroots to which can then be distributed to other webserver's filesystems keeping all our webroots in sync in short order  

```ssl/``` - this directory holds information to do with the SSL certificates, including the SSL certificates themselves which can then be distributed to all the webservers once updated on one of them  

```dbp.dat``` - the database prefix

```dbe.dat``` - marker file to remind us of which database the original install was made to

```joomla_configuration.php wordpress_config.php drupal_settings.php moodle_config.php``` - the various configuration files for our applications

```${HOME}/runtime/mariadb-init/initialiseDB-user.sql``` - customised sql script for creating a user during mysql DB initialisation using mariadb

```${HOME}/runtime/mariadb-init/initialiseDB.sql``` - customised sql script for creating and initialising a database during  database initialisation using mariadb

```${HOME}/runtime/mysql-init/initialiseDB-user.sql``` - customised sql script for creating a user during mysql DB initialisation using mysql

```${HOME}/runtime/mysql-init/initialiseDB.sql``` - customised sql script for creating and initialising a database during  database initialisation using mysql


```${HOME}/runtime/postgres-init/initialiseDB.psql``` - customised psql script for creating a user during mysql DB initialisation

```${HOME}/runtime/mysql-init/initialiseDB.psql``` - customised psql script for creating and initialising a database during  database initialisation



