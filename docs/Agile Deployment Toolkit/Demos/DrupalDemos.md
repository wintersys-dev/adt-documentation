#### MANDATORY PRE-REQUISITE STEPS (NEEDED BY ALL DEMOS BELOW)  

Perform step 1 or 2 below according to your experience and apply the overrides to your StackScript as described below for your desired demo type before you click "Create Linode"

1. If you are a beginner, follow [here](./QuickStartDemosPrepBeginnerLevel.md)  
2. If you are an expert, follow [here](./QuickStartDemosPrepExpertLevel.md)

-------------------------

#### QUICK DEMO OVERRIDE EXAMPLES

Once you have performed the mandatory steps above you can action specific demos by overriding the mentioned settings in the StackScript before you deploy it. By overriding different settings as described below, you will deploy different application types using the same StackScript.  

### Demo 1 (StackScript overrides for a baselined Drupal Version)


1. Once the application is installed, the username is "webmaster" and the password is "mnbcxz098321QQQZZZ"  
 
>     set "The Display name for your website e.g. My Demo Website" to "My Drupal Demo"  
>     set "APPLICATION" to "drupal"  
>     set "BASELINE DB REPOSITORY" to "drupal11.1.7-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "drupal11.1.7-webroot-sourcecode-baseline" 


If you are using the cloud-init method raher than StackScript these you should set

>     export WEBSITE_DISPLAY_NAME="My Drupal Demo"
>     export APPLICATION="drupal"  
>     export BASELINE DB REPOSITORY="drupal11.1.7-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="drupal11.1.7-webroot-sourcecode-baseline"


