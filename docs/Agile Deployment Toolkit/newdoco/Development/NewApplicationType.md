### INTEGRATING A NEW CUSTOM APPLICATION

If you want to integrate a new application type such as (for example) "NextCloud" or "Elgg" then this toolkit is structured to make that as easy as possible. There's a good chance there will be specific quirks with any application that you want to integrate because they are all a little different but I have endeavoured to structure this project as best I can to make it possible for you to accomodate any quirky behaviours you encounter. 

The structure of how an application is to be integrated works like this

-----------------------------------

#### Build Machine Scripts

On the Build Machine you will need to provide various scripts in support of your new application type and you can follow the design style/pattern that I have used for other application such as Joomla or Wordpress.

List of Build Machine scripts that need to be provided for your new bespoke application

>     application/SetHeadFile.sh
Here you will need to set the name of the index file for your application. This will mostly always be index.php but its possible that some applications may have a differently named entry point. 

>     application/cms|3rdparty/<application>/SetApplicationConfig.sh

This is where you will set the various dynamically allocated parameters in your descriptor. The reason why you have to dynamically allocate values to your descriptor is that some values that are held by the descriptor might be senstive such as password and usernames and so on and you don't want those to be hardcoded into a repository that you might also want to be public. The best example to look at is probably the joomla script which you can find at ${BUILD_HOME}/application/cms/joomla/SetApplicationConfig.sh

>     application/cms|3rdparty/<application>/descriptor.dat

The descriptor holds the configuration settings that you want to apply to the application you are deploying. This descriptor will be passed to the webserver machine when it is being built and the settings will be used to configure the application you are installing. 

>     processing/PostProcessingMessages.sh

If there are an post processing messages specific to the application you are installing that need to be conveyed to the deployer then you can put them in the PostProocessing script

>     specifications/quick_specification.dat
>     specifications/specification.md

You need to update the specifications in your fork to reflect the fact that your fork now supports a new application type

>     templatedconfigurations/ValidateTemplate.sh

Put any validation checks that can help when people are making an Expedited Build deployment that might help to keep then straight. 

-----------------------------

#### Webserver Scripts

On the websever machine you will need to provide the following scripts for the new application that you want to integrate into the toolkit. The scripts I have provided show a basic pattern for how you will want to go about implementing support for a new application type but your application will no doubt have quirks that make the patterns I have used similar or at least good guidance but not a step by step process for how to implement support for a new bespoke application type.

>     application/installation/cms|3rd-party/<application>/InstallVirginApplication.sh
You need to provide this script as a way to install your application. You might want to use checsums if they are avaialble. Sometime CLI tools for your application type can help with the installation and its often also possible in a lot of cases to clone git repos as a way of downloading your application sourcecode. The applications I provide support for have examples of the different approaches to the installation processes. Drupal for example uses composer, wordpress uses the wordpress CLI tool and so on. 

>     application/configuration/cms|3rd-party/<application>/InitialiseApplicationConfiguration.sh
This is likely the most involved script that you need to provide to integrate a new application. Before this runs the system will have downloaded the application and the application will then need to be initialised for your particular use case which is what this script does. The examples I have provided give a general pattern to how to proceed but different applications will like each have their own idiosyncracies. Pay attention to whether there is a CLI tool provided with your application that can help you otherwise you will likely have to do some manual intialising. 


>     application/monitoring/cms|3rd-party/<application>/CheckIfApplicationIsInstalled.sh
This script simply looks at what files are expected to be on the file system for a given application type. The list of files to expect can be set in your descriptor and if all the files and directories that the descriptor specifies as needing to be present are present on the file system then the application is considered to be fully present on the filesystem of your webserver. Its just a way of checking that we are good to go. 

>     services/cron/ExecuteApplicationSpecificCronjob.sh
If your application has any specific cron jobs that need to be run periodically then you can put them in this script with a suitable periodicity for invocation

>     utilities/security/EnforcePermissions.sh
If there's an application specific persmissions quirks that need to be set you can put add them here but most likely you won't need to do anything with this. 


>     webserver/configuration/application/nginx/site-available.conf.<APPLICATION>
>     webserver/configuration/application/apache/site-available.conf.<APPLICATION>
>     webserver/configuration/application/lighttpd/lighttpd.conf.<APPLICATION>

These are the webserver configurations for each application type. I am NOT an expert in webserver configurations so I provided bare bones scripts by default which can definitely be improved on. So, if you want to improve on the default configurations in your fork you are free to do so. Also, the scripts I have provided are about showing a pattern for how to support different application types. Most of what I do is about design rather than detailed technical refinements. 

--------------------------------------

#### Autoscaler Scripts

On the autoscaler machine you will need to have two scripts available to to support your new application. They are straight forward and self explanatory:

>     autoscaler/DoubleCheckConfig.sh
>     autoscaler/SelectHeadFile.sh
