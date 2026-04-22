### MANDATORY PRE-REQUISITE STEPS (NEEDED BY ALL DEMOS BELOW)

Perform step 1 or 2 below according to your experience and apply the overrides to your StackScript as described below for your desired demo type before you click "Create Linode"

<span style="color:red">1. IF YOU ARE A BEGINNER, follow [here](./QuickStartDemosPrepBeginnerLevel.md)</span>  
<span style="color:red">2. IF YOU ARE AN EXPERT (any experienced techie), follow [here](./QuickStartDemosPrepExpertLevel.md)</span>

-------------------------  

### QUICK DEMO OVERRIDE EXAMPLES

Once you have performed the mandatory steps above you can action specific demos by overriding the mentioned settings in the StackScript before you deploy it. By overriding different settings as described below, you will deploy different application types using the same StackScript. 

### Demo 1 (StackScript overrides for a virgin installation of the Joomla CMS)  

Set these fields of your StackScript as shown to deploy a copy of Joomla. The rest of the "Advanced Settings" can be set with their default values. You will need to set password, vpc, firewall and so on at the bottom of the script before you click "Create Linode". 

![](images/joomla-virgin.png "Joomla Install Screen") 

Go to the URL of your virgin Joomla installation in my case:

>     https://www.nuocial.uk

The Default username is "adt-webmaster" and the default password is the "ISGYNS2RXBR0"

---------------------------

### Demo 2 (StackScript overrides for a virgin installation of the Wordpress CMS)   

Set these fields of your StackScript as shown to deploy a copy of Wordpress. The rest of the "Advanced Settings" can be set with their default values. You will need to set password, vpc, firewall and so on at the bottom of the script before you click "Create Linode". 

![](images/wordpress-virgin.png "Wordpress Install Screen") 

Go to the URL of your virgin Wordpress installation in my case:

>     https://www.nuocial.uk

The Default username is "adt-webmaster" and the default password is the "ISGYNS2RXBR0"

---------------------------

### Demo 3 (StackScript overrides for a virgin installation of Drupal) 

Set these fields of your StackScript as shown to deploy a copy of Drupal. The rest of the "Advanced Settings" can be set with their default values. You will need to set password, vpc, firewall and so on at the bottom of the script before you click "Create Linode". 

![](images/drupal-virgin.png "Drupal Install Screen") 

Go to the URL of your virgin Wordpress installation in my case:  

>     https://www.nuocial.uk

The Default username is "adt-webmaster" and the default password is the "ISGYNS2RXBR0"

Advanced: 

NOTE: If you are interested in deploying Drupal CMS you need to fork the toolkit repositories and set the infrastructure repositories to your fork in the Stackscript rather than wintersys-dev and change the application descriptor for drupal in your fork to deploy drupal CMS

The application descriptor is at 

>     ${BUILD_HOME}/application/cms/drupal/descriptor.dat

Then to deploy "drupal CMS" you need to follow the exact same steps you just just followed for drupal but because you commented drupal and uncomented drupal cms in the descriptor, drupal CMS will be installed.

------------------
![](images/comment-drupal.png "Comment Drupal in descriptor")   
------------------
![](images/uncomment-drupal-cms.png "Uncomment Drupal CMS in descriptor") 
---------------------------

### Demo 4 (StackScript overrides for a virgin installation of the Moodle CMS)  

Set these fields of your StackScript as shown to deploy a copy of Moodle. The rest of the "Advanced Settings" can be set with their default values. You will need to set password, vpc, firewall and so on at the bottom of the script before you click "Create Linode". 


![](images/moodle-virgin.png "Moodle Install Screen") 

Go to the URL of your virgin Moodle installation in my case:

>     https://www.nuocial.uk

The Default username is "adt-webmaster" and the default password is the "ISGYNS2RXBR0"

---------------------------

### Demo 5 (StackScript overrides for a virgin installation of Open Source Social Network)

Set these fields of your StackScript as shown to deploy a copy of OSSN. The rest of the "Advanced Settings" can be set with their default values. You will need to set password, vpc, firewall and so on at the bottom of the script before you click "Create Linode". 

![](images/ossn-virgin.png "OSSN Install Screen") 

Go to the URL of the installation URL for OSSN in my case:

>     https://www.nuocial.uk/installation/index.php

Fill in the form with your username and password that you desire as well as the other details and then you should be able to login.

--------------------------------------------
--------------------------------------------

###Deploying virgin versions of the applications from baselined repositories.   

I put this here to show you how you can set up a baseline for your application type where you have installed a set of base plugins or extensions and repeatedly deploy that baseline an infinite number of time. As an example under the Wordpress installation I installed "wp-smtp-mail" and if you look you will see that that is available for you by default when you install the Wordpress baseline from the below and you can do the same thing with as many plugins as you like. I will provide more detailed examples later on where you can have whole bespoke social networks baselined and installable an infinite number of times. 

### Demo 6 (StackScript overrides for a virgin installation of the Joomla CMS from a baselined repository)  

![](images/virgin-joomla-baseline.png "Virgin Joomla Baseline Installation") 

Wait for the application install to have been completed and available at:

>      https://<dns-url>

The Default username is "adt-webmaster" and the default password is the "ISGYNS2RXBR0"

-----------------

### Demo 6 (StackScript overrides for a virgin installation of the Wordpress CMS from a baselined repository)  

![](images/virgin-wordpress-baseline.png "Virgin Wordpress Baseline Installation") 

Wait for the application install to have been completed and available at:

>      https://<dns-url>

The Default username is "adt-webmaster" and the default password is the "ISGYNS2RXBR0"

-----------------

### Demo 7 (StackScript overrides for a virgin installation of the Drupal CMS from a baselined repository)  

This is a sample virgin drupal installation from baselined repositories.  

1. Once the application is installed, the username is "webmaster" and the password is "mnbcxz098321QQQZZZ"


>     set "The number (1, 2 or 3) of the template you are using" to "2"  
>     set "The Display name for your website e.g. My Demo Website" to "My Vanilla Drupal Installation"  
>     set "APPLICATION" to "drupal"  
>     set "BASELINE DB REPOSITORY" to "drupal11.1.7-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "drupal11.1.7-webroot-sourcecode-baseline"

If you are using the cloud-init method raher than StackScript these you should set

>     export SELECTED_TEMPLATE="2"
>     export WEBSITE_DISPLAY_NAME="My Vanilla Drupal Installation"
>     export APPLICATION="drupal"
>     export BASELINE DB REPOSITORY="drupal11.1.7-db-baseline"
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="drupal11.1.7-webroot-sourcecode-baseline" 

Wait for the application install to have been completed and available at:

>      https://<dns-url>

NOTE: If you get an error message "The website encountered an unexpected error. Try again later" from Drupal CMS once it is installed you need to "clear all caches" which you can do by running

>     ${BUILD_HOME}/helpers/TruncateDrupalCache.sh

on your new build machine.

-----------------

### Demo 8 (StackScript overrides for a virgin installation of the Moodle CMS from a baselined repository) 

This is a sample virgin moodle installation from baselined repositories.  

1. Once the application is installed, the username is "webmaster" and the password is "mnbcxz098321QQQZZZ$$"


>     set "The number (1, 2 or 3) of the template you are using" to "2"  
>     set "The Display name for your website e.g. My Demo Website" to "My Vanilla Moodle Installation"  
>     set "APPLICATION" to "moodle"  
>     set "BASELINE DB REPOSITORY" to "moodle5.0-db-baseline" 
>     set "APPLICATION BASELINE SOURCECODE REPOSITORY" to "moodle5.0-webroot-sourcecode-baseline"

If you are using the cloud-init method raher than StackScript these you should set

>     export SELECTED_TEMPLATE="2"
>     export WEBSITE_DISPLAY_NAME="My Vanilla Moodle Installation"
>     export APPLICATION="moodle"
>     export BASELINE DB REPOSITORY="moodle5.0-db-baseline"
>     export APPLICATION BASELINE SOURCECODE REPOSITORY="moodle5.0-webroot-sourcecode-baseline" 

Wait for the application install to have been completed and available at:

>      https://<dns-url>
