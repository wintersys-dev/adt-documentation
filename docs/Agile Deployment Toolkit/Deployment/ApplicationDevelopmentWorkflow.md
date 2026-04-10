Rework this

deploy virgin application
develop your application
make a baseline of your application
make a test deployment of your baseline
make temporal (hourly) backup of the application that you deployed from the baseline with PERSIST_ASSETS_TO_DATASTORE set to off
you can then deploy from your temporal (hourly) backup with PERSIST_ASSETS_TO_DATASTORE set to on

-----------------------------


If you want to have a fully deployed application then the workflow will be as follows:

1. Deploy a virgin copy of your chosen CMS system and develop your application until you are happy with it.
2. Create baseline repositories of your application's webroot and database
3. Deploy the baselines you created in 2. using a domain name of your choice (you need to set

>     PERSIST_ASSETS_TO_DATASTORE=1

and 

>     DIRECTORIES_TO_MOUNT=<directory name>

if you want your assets to be persisted to the datastore automatically).   

4. Make a temporal backup of your applications webroot and database (this will most likely be an HOURLY periodicity backup).
  
5. Make a full application deployment using the temporal backups created in 4.


NOTE: It may well be the case that another developer or even company performs steps 1 and 2 for you and that you use the baselines they have developed for your own usecase. 
