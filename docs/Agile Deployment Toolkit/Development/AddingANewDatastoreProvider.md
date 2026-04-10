To add a new datastore manipulation tool such as rclone or s5cmd in addition to the s3cmd that the core supports you will need to modify or add to the following files:

>     adt-autoscaler-scripts/installation/InstallDatastoreTools.sh
>     adt-autoscaler-scripts/services/datastore/InitialiseDatastoreConfig.sh

>     adt-build-machine-scripts/configuration/software.dat
>     adt-build-machine-scripts/initialisation/InitialiseDatastoreConfig.sh
>     adt-build-machine-scripts/installation/InstallDatastoreTools.sh
>     adt-build-machine-scripts/services/datastore/*

>     adt-database-scripts/installation/InstallDatastoreTools.sh
>     adt-database-scripts/services/datastore/*

>     adt-webserver-scripts/installation/InstallDatastoreTools.sh
>     adt-webserver-scripts/services/datastore/*

