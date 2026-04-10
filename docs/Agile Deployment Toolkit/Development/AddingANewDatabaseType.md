To support a new database engine type you will need to modify or add to the following files. You might, for example want to integrate percona or mongodb and so on.

>     adt-autoscaler-scripts/autoscaler/InitialiseCloudInit.sh

>     adt-build-machine-scripts/configuration/software.dat
>     adt-build-machine-scripts/initialisation/InitialiseCloudInit.sh
>     adt-build-machine-scripts/initialisation/InitialiseDatabaseService.sh
>     adt-build-machine-scripts/application/moodle/SetApplicationConfig.sh
>     adt-build-machine-scripts/services/dbaas/AdjustDBaaSFirewall.sh
>     adt-build-machine-scripts/templatedconfigurations/quick_specification.dat
>     adt-build-machine-scripts/templatedconfigurations/specification.md

>     adt-database-scripts/application/db/*
>     adt-database-scripts/installation/InstallDatabaseClient.sh
>     adt-database-scripts/installation/InstallDatabaseServer.sh
>     adt-database-scripts/services/database/*
>     adt-database-scripts/services/utilities/remote/*
>     adt-database-scripts/services/utilities/status/IsDatabaseUp.sh

>     adt-webserver-scripts/installation/InstallDatabaseClient.sh
>     adt-webserver-scripts/installation/InstallMySQLClient.sh
>     adt-webserver-scripts/application/configuration/drupal/InitialiseVirginInstall.sh
>     adt-webserver-scripts/application/configuration/joomla/InitialiseVirginInstall.sh
>     adt-webserver-scripts/application/configuration/moodle/InitialiseVirginInstall.sh
>     adt-webserver-scripts/application/configuration/wordpress/InitialiseVirginInstall.sh
>     adt-webserver-scripts/services/utilities/remote/ConnectToRemoteMySQL.sh
>     adt-webserver-scripts/services/utilities/status/CheckServerAlive.sh
>     adt-webserver-scripts/ws.sh
