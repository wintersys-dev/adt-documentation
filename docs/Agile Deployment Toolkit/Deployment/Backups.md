**NOTE:** 

You can also use your cloudhost's backup service to make backups of your machines if you want super safe backups above and beyond what is provided here. Using your provider's backup service will usually incur extra costs but it should give you the ability to recover in the case of an absolute disaster. 

**NOTE1:**  

You can make multi-region backups simply by configuring the value for S3_HOST_BASE to be multi-region resilient by providing multi-region values for it in your template. For example if you set:  

**S3_HOST_BASE="nl-ams-1.linodeobjects.com:us-southeast-1.linodeobjects.com:in-maa-1.linodeobjects.com"**  

Then 3 backups will be made, one to the **nl-ams-1** region, one to **us-southeast-1** region and one to **in-maa-1** region. What this means is that you then have multi-region resilence for your webroot and database backups just by setting additional regions as shown above.

**BACKUP PERIODICITY** 

The backup periodicity is as follows:

##### hourly, daily, weekly, monthly, bimonthly 

What this means is that backups of the webroot and your database will be automatically initiated by cron at these different periodicities.

You can make a manual backup on from the build machine by runining the backup scripts from the build machine: 

>     ${BUILD_HOME}/helpers/PerformWebsiteBackup.sh 

>     ${BUILD_HOME}/helpers/PerformDatabaseBackup.sh  
 

---------------------------------------------------------------------------------------------------------

**HOW TEMPORAL BACKUPS ARE MADE FROM CRON**

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

You pass in the build periodicity **"HOURLY", "DAILY", "WEEKLY", "MONTHLY", "BIMONTHLY", "SHUTDOWN" or "MANUAL"** and also the **BUILD_IDENTIFIER** and that will then create a backup in your S3 datastore. The backup will be identifiable by "BUILD_IDENTIFIER" and "periodicity" in the datastore.   

-------------------------------------------------------------------------------------------------------------

**IF THE APPLICATION IS CONFIGURED TO STORE ASSETS IN THE CLOUD AND THEN MOUNTED THEY ARE NOT PART OF THE BACKUPS (PERSIST_ASSETS_TO_DATASTORE=1)**
	
Its normal (and required) to set 

>     PERSIST_ASSETS_TO_DATASTORE to 0

for baselines and virgin builds. This is because the cloud is only used to offload assets for a PRODUCTION class build.  

So ordinarily if your application users are going to be generating assets you want them to be stored in your datastore and distributed from there (see elsewhere in this doco). Note, if your assets are stored in the cloud i.e. PERSIST_ASSETS_TO_DATASTORE is set to 1, then, it is the only place where those assets are stored, there aren't any backups, so if you were to delete the assets from the bucket they are stored in by mistake, for example, it might hose your application. Therefore it is up to you to set up a backup policy for the assets that are stored in you S3 bucket. Its just a bucket with assets in it at the end of the day, so its not hard to make backups if you want to but some applications can generated GBs and GBs of assets and so can be hard to backup. Bottom line, be very cautious deleting image and media assets that are stored in S3 when PERSIST_ASSETS_TO_DATASTORE is set to 1 because by default they are the only copy you have of those assets. 
