#### DEVELOPING THE BUILD MACHINE SCRIPTS  

On your build machine installation you will find a clone of your live **&#0036;BUILD_HOME** adt-build-machine-scripts directory in the **/home/development** directory.
This (/home/development) is where you can make changes to the scripts of the build machine and when you are ready push them to your github repository branch or sync them to your live **&#0036;BUILD_HOME** area.  

The workflow is  

1. Edit and or add/delete scripts to **/home/development**
3. When you are happy with your script alterations, you have two options

- sync the changes you have made to your live **&#0036;BUILD_HOME** area for testing
- push the changes you have made to your github branch

There are three scripts which help you do this easily they are called  

- /usr/sbin/sync - this script will sync your development scripts (/home/development) with your live scripts located at **&#0036;BUILD_HOME**    

- /usr/sbin/push - this script will push the modifications you have made to your development scripts to your github branch you call this with a commit message - **/usr/sbin/push "commit message 1"**    

- /usr/sbin/push-and-sync - this script will perform the push to git and the sync to the live directories in **&#0036;BUILD_HOME** - **/usr/sbin/push-and-sync "commit message 1"**   

This then is suggested workflow if you want to enhance and alter the build machine scripts. You should never push to the main branch but instead to a dev branch and if you want to update the main branch issue a pull request in the GUI system of your git provider

--------------------------

#### DEVELOPING THE AUTOSCALER, WEBSERVER and DATABASE SCRIPTS  

To develop new code for these machine types you have to have a development instance of each machine type running and the structure and process is the same for each machine type as for the build-machine example above. The same helper scripts exist for each machine type and the process is the same independent of which particular machine type you are developing for. As a reminder the three scripts are:

- /usr/sbin/sync - this script will sync your development scripts (/home/development) with your live scripts located at **&#0036;HOME**    

- /usr/sbin/push - this script will push the modifications you have made to your development scripts to your github branch you call this with a commit message - **/usr/sbin/push "commit message 1"**    

- /usr/sbin/push-and-sync - this script will perform the push to git and the sync to the live directories in **&#0036;HOME** - **/usr/sbin/push-and-sync "commit message 1"**

NOTE: obviously you are not limited to using these scripts to interact with your git provider but I did write these scripts to try and help with what is likely the most common behaviours that are likely to be wanted. 
