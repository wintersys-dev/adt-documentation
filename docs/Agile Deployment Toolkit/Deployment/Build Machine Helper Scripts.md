#### Helper Scripts

On the build machine there is a set of helper scripts:

>     ${BUILD_HOME}/helperscripts/backup/DeleteRestorationArchives.sh

This script will delete the archives that you tell it to that have expired or are no longer needed. This toolkit generates a lot of backups that can be used to restore your application from if disaster strikes. Sometimes you will want to delete a subset of these archives to keep your datastore manageable and when you want to do that you can run this script. 

>     ${BUILD_HOME}/helperscripts/backup/EmergencyRestoration.sh

If disaster stikes and your website is either corrupted or hacked or has some other functional issue that you can't resolve due to a borked update or something you can quickly roll back to a previous archive from before the problem arose and perform an emergency restoration from that archive. 

>     ${BUILD_HOME}/helperscripts/backup/GenerateLocalBackups.sh

This will generate a backup of your website and database and store them on your local backup machine so that they are easily accessible

>     ${BUILD_HOME}/helperscripts/backup/PerformDatabaseBackup.sh

This script will perform a backup of your database for you it will be stored and accessible in your datastore under the url of your website along with the periodicity of the backup itself

>     ${BUILD_HOME}/helperscripts/backup/PerformWebsiteBackup.sh

This script will perform a backup of your webroot for you it will be stored and accessible in your datastore under the url of your website along with the periodicity of the backup itself

----------------------------------

>     ${BUILD_HOME}/helperscripts/baseline/PerformDatabaseBaseline.sh

This script will generate a baseline of your database for you that you can then deploy from. An empty repository of the naming convention "<identifier>-db-baseline" needs to have been created.


>     ${BUILD_HOME}/helperscripts/baseline/PerformWebsiteBaseline.sh

This script will generate a baseline of your webroot for you that you can then deploy from. An empty repository of the naming convention "<identifier>-webroot-sourcecode-baseline" needs to have been created.

-----------------------------------

>     ${BUILD_HOME}/helpers/datastore/CleanupDatastore.sh

You can run this script if you want to clean up your datastore. BE CAUTIOUS because you may delete buckets that are needed and or are not associated with this toolkit if you don't keep a tight rein on how you are using this. The point is, after multiple deployments you will likely want to clean up at the very least the hangover authentication buckets that this toolkit generates. 

----------------------------------

>     ${BUILD_HOME}/helpers/development/InitialiseToolkitForDevelopment.sh

If you develop and extend or maintain this toolkit it might be easier to simply make your updates directly in the GUI system editor of your git provider, but, if you want to make changes to the infrastructure in vi or nano and then test and push them to git rather than make your edits directly in the GUI editor then you will need to run this script. 


>     ${BUILD_HOME}/helpers/development/PushAndSyncInfrastructureScriptsUpdates.sh

This script will push and sync updates to the toolkit to your git repository

>     ${BUILD_HOME}/helpers/developmentPushInfrastructureScriptsUpdates.sh

This script will push updates to your git repository

>     ${BUILD_HOME}/helpers/development/SyncInfrastructureScriptsUpdates.sh

This script will sync the scripts in the development area with the live scripts area

---------------------------

>     ${BUILD_HOME}/helpers/security/AuditSSHConnections.sh

This can be run periodically from cron to inform us of any unrecognised SSH connections to our build machine. 

>     ${BUILD_HOME}/helpers/security/ManuallyUpdateSSLCertificate.sh

This can be run daily from cron to check if the SSL ceritificates that a particular build has deployed need to be regenerated or reissued (in other words, have they expired). If they have expired, then, this script will generate a new SSL certificate and install it on the webservers or reverse proxies and also authenticator machines if necessary. 

>     ${BUILD_HOME}/helpers/security/VirusScan.sh

This script can be run periodically to check for viruses. The reason for this is to protect windows users because viruses that can infect windows can still be relayed through a linux machine according to my understanding anyway? Correct me if I am wrong and this is a waste of time.

----------------------------


>     ${BUILD_HOME}/helpers/servers/ConnectToRemoteMachine.sh

This script will let you start an SSH session onto a particular remote machine. 

>     ${BUILD_HOME}/helpers/servers/CopyFromRemoteMachine.sh

This script will copy a file from a particular remote machine to our local build machine

>     ${BUILD_HOME}/helpers/servers/CopyToRemoteMachine.sh

This script will copy a file to remote machine(s) from our build machine. If you need a file copied to each machine or a subset of machines or an individual machine you can do that with this script. This might be useful if you were migrating  a codebase to the ADT form another provider, for example rather than messing about with FTP or scp.

>     ${BUILD_HOME}/helpers/servers/ExecuteOnRemoteMachine.sh

With  this script you can execute a command on remote machine(s). If you wanted to try this out you  could run it

>     /bin/sh ${BUILD_HOME}/helpers/servers/ExecuteOnRemoteMachine.sh "ls -l"

and run that command across multiple machines in sequence. 

------------------------------

>     ${BUILD_HOME}/helpers/services/AdjustScaling.sh

This  can be used to adjust the scaling  configuration of your server fleet (in other words, to adjust how many webservers are running  by entering  a lower number to scale down and a higher number to scale up). 

>     ${BUILD_HOME}/helpers/services/AllowLaptopIP.sh

This script enables you to grant access through the firewall to (for example) your colleague's laptop. It might be a bit of a faff but limiting access to only particular IP addresses is a good way to close of quite a few attack vectors. 

>     ${BUILD_HOME}/helpers/services/DisplayLoggingStreams.sh 

This will display log output either error logs or standout logs

>     ${BUILD_HOME}/helpers/services/DisplayPassword.sh

This will display the SERVER_USER_PASSWORD variable in other words, the password for your machines

>     ${BUILD_HOME}/helpers/services/GetBuildMachineIP.sh

This will tell you the build machine's IP address

>     ${BUILD_HOME}/helpers/services/GetOsName.sh

This gets the name of the OS we are running on

>     ${BUILD_HOME}/helpers/services/GetVariableValue.sh

This gets the valuable of a variable from the build environment

>     ${BUILD_HOME}/helpers/services/IsHardcoreBuild.sh

This will return true if the current build was initiated as a hardcore build

>     ${BUILD_HOME}/helpers/services/RunServiceCommand.sh

This allows us to run various service commands like stopping and starting ssh or stopping  and starting any other service as the need arises

>     ${BUILD_HOME}/helpers/services/SetVariableValue.sh

Thiis will set a variable in the build environment
