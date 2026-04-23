**EXPEDITED TEMPORAL DEPLOYMENT METHOD**

YOU MUST HAVE THE MACHINES STILL RUNNING FROM THE PREVIOUS [TUTORIAL](./expedited-baseline-joomla.md) TO COMPLETE THIS TUTORIAL SUCCESSFULLY

On your build machine, you need to now make hourly periodicity [temporal backups](../../Deployment/Backups.md) of your application sourcecode and your application database.

To generate your temporal backups, you have to run two commands on your build machine. At the command prompt of your build machine cd into the **helpers** directory of your agile deployment toolkit installation. In my case it is like this:

>     cd /home/wintersys-dev/adt-build-machine-scripts/helpers

Then perform a temporal database backup
  
>     /bin/sh PerformDatabaseBackup.sh
 
Answering all of the questions and picking a periodicity, for example, HOURLY in your template. Check in your datastore for a database temporal backup which should look similar to this picture with an appropriate timestamp for you.

![](images/expedited/do-52.png "Digital Ocean Tutorial Image 52")

  
Then perform a temporal website sourcecode backup
  
>     /bin/sh PerformWebsiteBackup.sh
  
Making sure you pick the same periodicity as for the temporal database backup, for example, "HOURLY"

![](images/expedited/do-51.png "Digital Ocean Tutorial Image 51")

  
What we are then interested in is template 3 which is at:
  
>     /home/wintersys-dev/adt-build-machine-scripts/templatedconfigurations/templates/digitalocean/digitalocean3.tmpl
  
I can extract the values for the following variables from template 1 or template 2 which I used in the previous tutorials in the series and set them in template 3, replace these with your own live values:

>     export S3_ACCESS_KEY="DO00YVU7YHGD1HHDJK" #MANDATORY
>     export S3_SECRET_KEY="eAlmwh3y7eJovZSN/TxSz7RZrsbdvjvburfr7whs" #MANDATORY
>     export TOKEN="dop_v1_51ac97f187fadb12ac70022521dbafh3f56259608fad0dadafbb594e2417ab78dba2" #MANDATORY
>     export DNS_USERNAME="peterdigitalocean@yahoo.com" #MANDATORY
>     export DNS_SECURITY_KEY="dop_v1_51ac97f187fadb12ac70022521dbafh3f56259608fad0dadafbb594e2417ab78dba2" #MANDATORY
>     export WEBSITE_DISPLAY_NAME="Test Social Network" #MANDATORY
>     export WEBSITE_NAME="drpatient" #MANDATORY - This is the exact value of the core of your WEBSITE_URL, for example, www.nuocial.org.uk would be nuocial
>     export WEBSITE_URL="www.drpatient.com"  #MANDATORY
>     export APPLICATION_REPOSITORY_OWNER="adt-apps" #MANDATORY
>     export APPLICATION_REPOSITORY_USERNAME="adt-apps" #MANDATORY
>     export APPLICATION_REPOSITORY_TOKEN="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k4" #MANDATORY
>     export VPC_IP_RANGE="10.106.16.0/20"
> 
What I then do is adjust  

**/home/wintersys-dev/adt-build-machine-scripts/templatedconfigurations/templates/digitalocean/digitalocean3.tmpl**  
  
to contain these values instead of its defaults.
  
I then need to set the template to use the temporal backups that I have generated and I do that by setting these values in template3:
  
>     export APPLICATION="joomla" #MANDATORY (joomla or wordpress or drupal or moodle)
>     export BUILD_ARCHIVE_CHOICE="hourly" #MANDATORY hourly, daily, weekly, monthly, bimonthly
>     export PERSIST_ASSETS_TO_DATASTORE="1" #MANDATORY This should only be 0 if your application has a very small number of assets
>     export DIRECTORIES_TO_MOUNT="images" #MANDATORY - this will define which directories in your webroot will be mounted from S3, if PERSIST_ASSETS_TO_DATASTORE=1
  
Shutdown any webservers that you have running from tutorial 2 and you are then ready to perform a temporal build, as shown below:
  
If your template is configured correctly you can now run the build process selecting the appropriate template and cloudhost (digitalocean and template 3 in other words). On your build machine, do as follows:

>     cd ${BUILD_HOME}
>     ./ExpeditedAgileDeploymentToolkit.sh

and answer any questions and select template 3 and have a bit of patience whilst the build runs. 

When I ran a temporal build process, I saw the following output from the ADT
![](images/expedited/do-53.png "Digital Ocean Tutorial Image 53")
![](images/expedited/do-54.png "Digital Ocean Tutorial Image 54")
![](images/expedited/do-55.png "Digital Ocean Tutorial Image 55")
![](images/expedited/do-56.png "Digital Ocean Tutorial Image 56")
![](images/expedited/do-57.png "Digital Ocean Tutorial Image 57")
![](images/expedited/do-58.png "Digital Ocean Tutorial Image 58")
![](images/expedited/do-59.png "Digital Ocean Tutorial Image 59")
![](images/expedited/do-60.png "Digital Ocean Tutorial Image 60")



Notice that there are two webservers and an autoscaler running because this is a temporal build
![](images/expedited/do-62.png "Digital Ocean Tutorial Image 62")
Notice that an images or assets bucket has been created in the datastore and is mounted within the webroot of the application as /var/www/html/images
![](images/expedited/do-63.png "Digital Ocean Tutorial Image 63")
And this is what my community builder example looks like deployed from a temporal backup
![](images/expedited/do-67.png "Digital Ocean Tutorial Image 67")
