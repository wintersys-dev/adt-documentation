The workflow that you need to follow to have a fully deployment is as follows.

1. Deploy a virgin copy of your CMS system and customise the CMS to create your application

3. Baseline your application so that you have a baselined copy of your database and your webroot held with your git provider

4. Take the original application offline and deploy from your new baseline to ensure that your basseline is working correctly. That baseline can then 
be the foundation for as many deployments of that application as you want   
(NOTE: remeber to switch on on PERSIST_ASSETS_TO_DATASTORE and DIRECTORIES_TO_MOUNT if needed in your template before you deploy your baseline)

5. Make a temporal backup (or wait for 'on the hour' for a backup to be made) - if you have PERIST_ASSETS_TO_DATASTORE set to "1" then when you make a   
temporal backup of your running baseline, the assets set by "DIRECTORIES_TO_MOUNT" will be persisted to the datastore.

6. You are then all set to easily make "full" deployments of your application but just remember to set
"PERSIST_ASSETS_TO_DATASTORE" and  "DIRECTORIES_TO_MOUNT"
to fit with what you set when you made the baseline. By persisting user assets to the datastore you can cope with very large numbers of user assets
as long as you have the finances to allow it because the asset sets are offloaded to the datastore rather than being  on the filesystem of your webserver(s).

NOTE: If you already have a baseline or you are using a baseline of an application provided by a 3rd party then you only need to perform from step 3 onwards in the steps listed above
