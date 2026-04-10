#### MANDATORY PRE-REQUISITE STEPS (NEEDED BY ALL DEMOS BELOW)

Perform step 1 or 2 below according to your experience and apply the overrides to your StackScript as described below for your desired demo type before you click "Create Linode"

1. If you are a beginner, follow [here](./QuickStartDemosPrepBeginnerLevel.md)  
2. If you are an expert, follow [here](./QuickStartDemosPrepExpertLevel.md)

-------------------------

#### QUICK DEMO OVERRIDE EXAMPLES

Once you have performed the mandatory steps above you can action specific demos by overriding the mentioned settings in the StackScript before you deploy it. By overriding different settings as described below, you will deploy different application types using the same StackScript.

### Demo 1 (StackScript overrides for a sample Wordpress application installation) 

This is just a sample wordpress template with some sample data installed which will show you how you can get a pre-built site up and running with this toolkit

1. Assuming that you have your valid credentials in your credentials file on your laptop (if you don't know what this is, go to the beginning of this document and start there) follow the steps in "POPULATE YOUR STACKSCRIPT" above.
2. Once the steps in POPULATE YOUR STACKSCRIPT have been followed fully, make the following additional changes to the advanced settings of your Stackscript.
 
>     set "The Display name for your website e.g. My Demo Website" to "My Wordpress Demo"  
>     set "APPLICATION" to "wordpress"  
>     set "BASELINE DB REPOSITORY" to "wordpressdemo-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "wordpressdemo-webroot-sourcecode-baseline"

If you are using the cloud-init method raher than StackScript these you should set

>     export WEBSITE_DISPLAY_NAME="My Wordpress Demo"
>     export APPLICATION="wordpress"  
>     export BASELINE DB REPOSITORY="wordpressdemo-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="wordpressdemo-webroot-sourcecode-baseline"
