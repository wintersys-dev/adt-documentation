

The most relevant parts of the configuration are the 

>     PERSIST_ASSETS_TO_DATASTORE

and 

>     DIRECTORIES_TO_MOUNT
>     SYNC_WEBROOTS

settings in your template.

- SYNC_WEBROOTS will exclude the directories your have defined in DIRECTORIES_TO_MOUNT from the syncing process and it is assumed that for such directories the authoritative store is the S3 bucket that is mounted there. 

- If you are deploying a virgin website installation or a baseline website installation then **PERIST_ASSETS_TO_DATASTORE** should always be 0.

- If you are deploying from a temporal backup then **PERSIST_ASSETS_TO_DATASTORE** can be 1.

- When **PERSIST_ASSETS_TO_DATASTORE** is 1, then you can set which directories of your applications webroot you wish to be stored in the cloud (the S3 Data storage). Please refer to the specification for more details on how to set the value of **DIRECTORIES_TO_MOUNT**. If **SYNC_WEBROOTS** is 1, the directories mounted from S3 will not be included in the syncing process.

--------

The workflow for the assets for the application are arranged as follows. 

As part of the preparation for the build process on the build machine when you run "ExpeditedAgileDeploymentToolkit.sh" when you build from a temporal backup, **PERSIST_ASSETS_TO_DATASTORE** is set to 1 and **DIRECTORIES_TO_MOUNT** has a value that matches the webroot structure of your application, then the assets will be copied from the temporal backup to the S3 datastore and mounted in place of the assets actually being on the webserver as part of your application.

Here is the workflow.

- You deploy from a baselined copy of your application (**PERSIST_ASSETS_TO_DATASTORE**) must be "0".

- You make an HOURLY backup from this active version of your website (because **PERIST_ASSETS_TO_DATASTORE**="0" the assets of the application will be stored as part of the backup)

- Once you have temporal backups of your website that contain all assets you take the active deployment that you made the backups from offline.

- You then confligure your system to deploy from a temporal backup setting **PERSIST_ASSETS_TO_DATASTORE** to "1" and DIRECTORIES_TO_MOUNT to (for example if you are deploying joomla) to "images"

- When you deploy from the temporal backup the build machine will make a copy of the assets from your temporal backup to the S3 datastore in an identifiable bucket (one bucket for each specific directory that you set for your application)

- The application is then deployed and on each webserver, the assets that were in the temporal backup that match to your "DIRECTORIES_TO_MOUNT" are deleted from your direct filesystem and instead are mounted from the identifiable bucket.

- When subsequent temporal backups are made of your webserver the directories that have been mounted from S3 will be omitted from the backup leaving only a single place of truth for your assets which is the identifiable bucket mentioned above. It is recommeneded therefore if you use this technique that you make backups of your assets manually (in other words, sync your assets to a secondary bucket in another region for safety reasons).

This means that the S3 capacity is your SHARED limit across all your webservers for how many assets you have and not the size of each webservers disk space.
