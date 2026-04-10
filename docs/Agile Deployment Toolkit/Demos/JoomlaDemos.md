#### MANDATORY PRE-REQUISITE STEPS (NEEDED BY ALL DEMOS BELOW)

Perform step 1 or 2 below according to your experience and apply the overrides to your StackScript as described below for your desired demo type before you click "Create Linode"

1. If you are a beginner, follow [here](./QuickStartDemosPrepBeginnerLevel.md)  
2. If you are an expert, follow [here](./QuickStartDemosPrepExpertLevel.md)

-------------------------

#### QUICK DEMO OVERRIDE EXAMPLES

Once you have performed the mandatory steps above you can action specific demos by overriding the mentioned settings in the StackScript before you deploy it. By overriding different settings as described below, you will deploy different application types using the same StackScript.


### Demo 1 (StackScript overrides for an example Joomla demo)

This is just a sample joomla install with some sample data installed 

1. Once the application is installed, the username is "webmaster" and the password is "mnbcxz098321QQQZZZ".
 
>      set "The Display name for your website e.g. My Demo Website" to "My Joomla Demo"  
>      set "BASELINE DB REPOSITORY" to "joomlademo-db-baseline" 
>      set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "joomlademo-webroot-sourcecode-baseline"


If you are using the cloud-init method raher than StackScript these you should set

>     export WEBSITE_DISPLAY_NAME="My Joomla Demo"
>     export BASELINE DB REPOSITORY="joomlademo-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="joomlademo-webroot-sourcecode-baseline"

---------------------------

### Demo 2 (StackScript overrides for an example Joomla Astroid demo)

[Astroid Framework](https://astroidframe.work/)

This is just a sample joomla install with the astroid framework installed

1. Once the application is installed, the username is "admin" and the password is "mnbcxz098321QQZZ"
 
>      set "The Display name for your website e.g. My Demo Website" to "My Astroid Demo"   
>      set "BASELINE DB REPOSITORY" to "astroid-db-baseline" 
>      set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "astroid-webroot-sourcecode-baseline"   

If you are using the cloud-init method raher than StackScript these you should set

>     export WEBSITE_DISPLAY_NAME="My Astroid Demo"
>     export BASELINE DB REPOSITORY="astroid-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="astroid-webroot-sourcecode-baseline"

----------------------------
### Demo 3 (StackScript overrides for an example Joomla Kunena demo)

[Kunena](https://www.kunena.org)

This is just a sample joomla install with the kunena forum installed

1. Once the application is installed, the username is "webmaster" and the password is "mnbcxz098321QQZZ"
 
>     set "The Display name for your website e.g. My Demo Website" to "My Kunena Demo"   
>     set "BASELINE DB REPOSITORY" to "kunena-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "kunena-webroot-sourcecode-baseline"   

If you are using the cloud-init method raher than StackScript these you should set

>     export WEBSITE_DISPLAY_NAME="My Kunena Demo"
>     export BASELINE DB REPOSITORY="kunena-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="kunena-webroot-sourcecode-baseline"
> 
----------------------------

### Demo 4 (StackScript overrides for example Joomlart demos) 

#### Demo JA Template One  

Username: webmaster  
Password: mnbcxz098321QQZZ  

[Joomlart Purity](https://www.joomlart.com/joomla/templates/ja-purity-iv)
This is just a sample joomla install with the purity template installed 

>     set "WEBSITE_DISPLAY_NAME" to "My Purity Demo"
>     set "BASELINE DB REPOSITORY" to "purity-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "purity-webroot-sourcecode-baseline"

If you are using the cloud-init method raher than StackScript these you should set

>     export WEBSITE_DISPLAY_NAME="My Purity Demo"
>     export BASELINE DB REPOSITORY="purity-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="purity-webroot-sourcecode-baseline"
> 

OR  

Username: webmaster
Password: mnbcxz098321QQZZ

#### Demo JA Template Two  
[Joomlart Stark](https://www.joomlart.com/joomla/templates/ja-stark)
This is just a sample joomla install with the stark template installed 

>     set "WEBSITE_DISPLAY_NAME" to "My Stark Demo"
>     set "BASELINE DB REPOSITORY" to "stark-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "stark-webroot-sourcecode-baseline"

If you are using the cloud-init method raher than StackScript these you should set

>     export "WEBSITE_DISPLAY_NAME" to "My Stark Demo"
>     export BASELINE DB REPOSITORY="stark-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="stark-webroot-sourcecode-baseline"


OR 

#### Demo JA Template Three  
[Joomlart Campaign](https://www.joomlart.com/joomla/templates/ja-campaign)
This is just a sample joomla install with the stark template installed 

>     set WEBSITE_DISPLAY_NAME="My Campaign Demo"
>     set "BASELINE DB REPOSITORY" to "campaign-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "campaign-webroot-sourcecode-baseline"    

If you are using the cloud-init method raher than StackScript these you should set

>     export WEBSITE_DISPLAY_NAME="My Campaign Demo"
>     export BASELINE DB REPOSITORY="campaign-db-baseline" 
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="campaign-webroot-sourcecode-baseline"

Once the application is installed, the username is "webmaster" and the password is "mnbcxz098321QQZZ"
