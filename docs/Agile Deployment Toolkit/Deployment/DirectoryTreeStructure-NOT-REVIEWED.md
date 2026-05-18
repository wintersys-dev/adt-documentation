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

```${HOME}/runtime/WEBSERVER_READY``` - This is a marker file that tells us when a webserver has fully completed its build process  

```${HOME}/runtime/datastore_workarea``` - This directory is what the datastore manipulation scripts can use as an adhoc work area

```${HOME}/runtime/application.dat``` - This is the configuration file for the currently installed application

```${HOME}/runtime/backupworkarea```  - This is a directory which scripts related to backups and baselines can use as an adhoc work area

```${HOME}/runtime/filesystem_sync``` - When filesystems are being synced between machines, this can be used as a data area to facilitate the syncing process

```${HOME}/runtime/CONFIG_EMAIL_SENT```  - When an application is installed this flag stops there being mulitple emails set if a config installation fails

```${HOME}/runtime/CONFIG_SITE_EMAIL_SENT``` - When an application is installed this flag stops there being mulitple emails set if a config installation fails

```${HOME}/runtime/INITIAL_CONFIG_SET``` - This flag tells us that the configuration of our current application has been completed successfully

```${HOME}/runtime/installedsoftware``` - Marker files for installed software are placed here, in other words, we can refer to this to see what software has been installed on our machine

```${HOME}/runtime/FIREWALL-ACTIVE``` - This marker file tells us that the firewall is considered active

```${HOME}/runtime/SSMTP_INITIALISED``` - This marker files tells us that the SMTP system has been made operational

```${HOME}/runtime/authenticator```  - When there is an authentication server this directory is used to faciliate the authentication process

```${HOME}/runtime/IP_FORWARDING_ENABLED```  - This marker file tells us that IP Forwarding has been enabled

```${HOME}/runtime/KNICKERS_ARE_UP```  - This marker file tells us that the firewall is blocking incoming connections by default

```${HOME}/runtime/FIREWALL-INITIALISED```  - This marker file means that the firewalling system is considered initialised

```${HOME}/runtime/FIREWALL-ACTIVE``` - This marker file tells us that the firewalling system is considered active

```${HOME}/runtime/firewall.dat``` - This is the configuration file related to the firewall

```${HOME}/runtime/software.dat``` - This file configures how the software we are installing should be installed, for example, should a particular software program be installed from source or from repository

```${HOME}/runtime/webserver_configuration_settings.dat``` - This file contains all the configuration settings for our current webserver

```${HOME}/runtime/SHUTDOWN-INITIATED``` - This marker file tells us that a Shutdown of the current webserver has been initiated

```${HOME}/runtime/otherwebserverips``` - This is where a list of all the webserver IPs that are active in the system are recorded except the current webserver IP address

```${HOME}/runtime/ssh-audit``` - This directory is where the ssh audit information is held

```${HOME}/runtime/virus_report``` - This directory contains any virus reports that might have been generated   

```${HOME}/runtime/INITIAL_BUILD_WEBSERVER_ONLINE``` - This is a marker file that tells us that our current webserver has completed its intial build

```${HOME}/runtime/AUTOSCALED_WEBSERVER_ONLINE```  - This is a marker file that tell us that the current webserver has been build as the result of an autoscaling event

```${HOME}/runtime/ATOP_RUNNING``` - This is a marker file which tells us that the ATOP process is running

```${HOME}/runtime/MARKEDFORSHUTDOWN``` - This marker file will be present if a machine is a candidate for being shutdown and terminated

```${HOME}/runtime/CPU_OVERLOAD_ACKNOWLEDGED``` - This marker file tells us that a CPU overload event has been acknowledged

```${HOME}/runtime/LOW_MEMORY_ACKNOWLEDGED``` - This is a marker file that tell us that a low memory state has been acknowledged

````${HOME}/runtime/LOW_DISK_ACKNOWLEDGED``` - This is a marker file that tells us that a low disk space state has been acknowledged

```${HOME}/runtime/BESPOKE_APPLICATION_INSTALLED``` - This marker file tells us that our application is considered installed

```${HOME}/cron``` - This directory contains the scripts relating to cron functionality

```${HOME}/installation``` - This directory contains all the scripts which relate to installation of software and so on

```${HOME}/system``` - This is where all scripts related to third party providers are kept such as datastore providers, git providers and so on

```${HOME}/security``` - This directory has scripts that relate to security such as the firewall  

```${HOME}/runtime/WEBSERVER_CONFIG_LOCATION.dat``` - this stores the location of the webserver configuration file for ease of access elsewhere

------------------------------

#### Database Machines

```${HOME}/runtime/datastore_workarea``` - this can be used by the scripts which interact with the datastore as a work area

```${HOME}/runtime/DB_APPLICATION_INSTALLED```  - this is a marker file which is set when  the application is successfully installed into the database

```${HOME}/runtime/DATABASE_SYSTEM_INSTALLED``` - This is a marker file which tells us that the core database system is installed

```${HOME}/runtime/DATABASE_READY``` - this is a marker file which shows us that the database machine is considered to have been fully built and all software to have been installed

```${HOME}/runtime/mysql-init``` - this is a directory for mysql intialisation scripts to be held

```${HOME}/runtime/postgres-init``` - this is a directory for postgres initialisation scripts to be held

```${HOME}/runtime/SSMTP_INITIALISED``` - this is a marker file which tells us that the SMTP system is installed and functional

```${HOME}/runtime/KNICKERS_ARE_UP```  - this shows us that any inbound connections to the machines ports have been blocked unless explicitly authorised

```${HOME}/runtime/FIREWALL-ACTIVE``` - this shows us that the firewalling system is considered active

```${HOME}/runtime/firewall.dat``` - this configuration file holds the configuration for the firewall

```${HOME}/runtime/mariadb-init/initialiseDB-user.sql``` - customised sql script for creating a user during mysql DB initialisation using mariadb

```${HOME}/runtime/mariadb-init/initialiseDB.sql``` - customised sql script for creating and initialising a database during  database initialisation using mariadb

```${HOME}/runtime/mysql-init/initialiseDB-user.sql``` - customised sql script for creating a user during mysql DB initialisation using mysql

```${HOME}/runtime/mysql-init/initialiseDB.sql``` - customised sql script for creating and initialising a database during  database initialisation using mysql

```${HOME}/runtime/postgres-init/initialiseDB.psql``` - customised psql script for creating a user during mysql DB initialisation

```${HOME}/runtime/mysql-init/initialiseDB.psql``` - customised psql script for creating and initialising a database during  database initialisation

```${HOME}/runtime/FIREWALL-INITIALISED``` - this marker file is set when the firewall is considered to have been initialised

```${HOME}/runtime/FIREWALL-ACTIVE``` - this marker file is set when the firewall has been made active

```${HOME}/runtime/software.dat``` - this configuration file holds the setting for the various softwares that the machine uses

```${HOME}/runtime/database_configuration_settings.dat``` - this configuration file holds the settings for the current database machine

```${HOME}/runtime/ssh-audit``` - any ssh connection audit reports are stored here

```${HOME}/runtime/virus_report``` - any virus scanning reports are stored here

```${HOME}/runtime/installedsoftware``` - this directory contains a list of marker files that show us which softwwares are considered to have been successfully installed

```${HOME}/runtime/ATOP_RUNNING``` - this is a marker file telling us that the TOP is running

```${HOME}/runtime/CPU_OVERLOAD_ACKNOWLEDGED``` - this is a marker file telling us that CPU overload status has had a notification email sent

```${HOME}/runtime/LOW_MEMORY_ACKNOWLEDGED``` - this is a marker file telling us that low memory status has had a notification email sent

```${HOME}/runtime/LOW_DISK_ACKNOWLEDGED``` - this is a marker file telling us that low disk status has had a notification email sent

```${HOME}/applicationdb``` - the files to do with the specific application we are installing

```${HOME}/cron``` - This directory contains the scripts relating to cron functionality

```${HOME}/installation``` - This directory contains all the scripts which relate to installation of software and so on

```${HOME}/system``` - This is where all scripts related to third party providers are kept such as datastore providers, git providers and so on

```${HOME}/security``` - This directory has scripts that relate to security such as the firewall  


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



