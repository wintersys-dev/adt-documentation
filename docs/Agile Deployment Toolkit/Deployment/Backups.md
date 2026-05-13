#### HOW TO GENERATE BESPOKE BACKUPS OF YOUR WEBROOT AND DATABASE

**NOTE:** 

You can also use your cloudhost's backup service to make backups of your machines if you want super safe backups above and beyond what is provided here. Using your provider's backup service will usually incur extra costs but it should give you the ability to recover in the case of an absolute disaster. 

**NOTE1:**  

You can make multi-region backups simply by configuring the value for S3_HOST_BASE to be multi-region resilient (please see Datastore Operational Protocols to see how to make your backups mutli-region). For example you would have to set **S3_HOST_BASE** as well as the S3 related settings to multi-region which will look something like this: 

**S3_HOST_BASE="nl-ams-1.linodeobjects.com:us-southeast-1.linodeobjects.com:in-maa-1.linodeobjects.com"**  

Then 3 backups will be made, one to the **nl-ams-1** region, one to **us-southeast-1** region and one to **in-maa-1** region. What this means is that you then have multi-region resilence for your webroot and database backups just by setting additional regions as shown above.

**BACKUP PERIODICITY** 

The backup periodicity is as follows:

##### hourly, daily, weekly, monthly, bimonthly 

What this means is that backups of the webroot and your database will be automatically initiated by cron at these different periodicities.

You can make a manual backup on from the build machine by runining the backup scripts from the build machine: 

>     ${BUILD_HOME}/helpers/backup/PerformWebsiteBackup.sh 

>     ${BUILD_HOME}/helpers/backup/PerformDatabaseBackup.sh  
 

---------------------------------------------------------------------------------------------------------

#### HOW TEMPORAL BACKUPS ARE MADE FROM CRON

The backups are created by calling the script

>     ${HOME}/cron/BackupFromCron.sh**

on the webserver machine  

and  

>     ${HOME}/cron/BackupFromCron.sh

on the database machine 

The cron script calls

>     ${HOME}/application/backup/Backup.sh

on the webserver machine  

and  

>     ${HOME}/application/backup/Backup.sh

on the database machine  

The build periodicity is passed to the script **"HOURLY", "DAILY", "WEEKLY", "MONTHLY", "BIMONTHLY", "SHUTDOWN" or "MANUAL"** and also the **BUILD_IDENTIFIER** and that will then create a backup in your S3 datastore. The backup will be identifiable by "BUILD_IDENTIFIER" and "periodicity" in the datastore.   

-------------------------------------------------------------------------------------------------------------

** ASSETS MOUNTED FROM S3**

If your assets are mounted from S3 into your webroot because you have configured

>     PERSIST_ASSETS_TO_DATASTORE to 1

and have set which directories you want to be mounted from S3 then you don't want those assets in your backup. So, the scripts automatically exclude whatever directories have been configured as mounted from the datastore. 

NOTE: Up to the point where you have baselined an application you are developing you certainly don't want to have PERSIST_ASSETS_TO_DATASTORE set to "1" because you will want the assets to be part of your baseline its only once you have a baseline to work from that you will want to start considering setting  PERSIST_ASSETS_TO_DATASTORE to "1"

