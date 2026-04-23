**EXPEDITED BASELINE DEPLOYMENT METHOD**

If you have followed the tutorial [here](./expedited-virgin-joomla.md), then you will have an active Joomla, installation active through your web browser.
What you need to do now is to customise your version of (Joomla) so that it is a specialised application for example a blog or a social network and so on. 

My suggestion is for you to test this sytem by installing Community Builder from Joomlapolis (you can customise it as little or as much as you want) into your new Joomla installation. "Community Builder" can be found here: [Community Builder](https://www.joomlapolis.com). 

**Take 5 minutes to install Community Builder onto your new Joomla application. Once you are happy with what you have built, you need to make a baseline of it.** The baseline will be written the github account that your template is set to use, in my example, it is my "adt-apps" github account yours will be something different. 

In order to create the baseline of my custom application, I need to do the following:

1. Choose a unique identifier for my baseline repositories, in this case I am going to call them, "communitybuilder" yours will be a different name.
2. Go to you git provider account console with your browser, in this case it is my "adt-apps" account with Github and create two **private** repositories with the following names:

>     communitybuilder-webroot-sourcecode-baseline
>     communitybuilder-db-baseline

In my case it looks like the following when I create these repos in my "adt-apps" github account:

![](images/expedited/exo26.png "Exoscale Tutorial Image 26")
![](images/expedited/exo27.png "Exoscale Tutorial Image 27")

Once these two repositories have been created (in my case in my adt-demos) account you are ready to make a baseline of the joomla install that you have modified. 

3. To generate your baseline, you have to run two commands on your build machine. At the command prompt of your build machine cd into the **helpers** directory of your agile deployment toolkit installation. In my case it is like this:

>     cd /home/wintersys-dev/adt-build-machine-scripts/helpers

Once you are in that directory, you need to issue the command:

>     /bin/sh PerformWebsiteBaseline.sh

Once that starts running, you need to answer the questions you are prompted for entering, "communitybuilder" (which must match the repository names you set above) when you are prompted for an identifier. 

In a minute or two your website baseline will have been generated and you should check in its repository that sourceode has been generated to it. 

![](images/expedited/exo28.png "Exoscale Tutorial Image 28")

Now you need to generate a baseline of the database. To do that you need to issue the command:

>     /bin/sh PerformDatabaseBaseline.sh

Once that starts running, you need to answer the questions you are prompted for entering, "communitybuilder" (which must match the repository names you set above) when you are prompted for an identifier. 

In short order, my database is backed up to the Github repository and again, I should check that the repository I have chosen has been updated using the github console.

![](images/expedited/exo29.png "Exoscale Tutorial Image 29")

-----------------------------------------------

My application baselines are now complete. The process for generating baselines is the same whichever application type you have built, Joomla, Wordpress, Drupal or Moodle. 

The next step is to make a deployment using these baselines. So, if I have any webservers or databases running with my cloudhost, I need to take them off line (shut them down) and destroy them. 

I am then interested in template 2 because that is the template that is used for deploying baselined application. If its not clear, template 1 is used for virgin CMS deployments, template 2 is used for baselined application deployments and template 3 is used for temporal deployments. 

So, template 2 is located here on my build machine:

>     /home/wintersys-dev/adt-build-machine-scripts/templatedconfigurations/templates/digitalocean/digitalocean2.tmpl

In this case because you have already configured template 1 you can crib most of the credentials from 

>     /home/wintersys-dev/adt-build-machine-scripts/templatedconfigurations/templates/digitalocean/digitalocean1.tmpl

and use them in digitalocean2.tmpl

I can extract the values for the following variables from digitalocean1.tmpl on my build machine and copy and paste them into the correct place in digitalocean2.tmpl:

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

There are some other values that I need to change in 

>     /home/wintersys-dev/adt-build-machine-scripts/templatedconfigurations/templates/digitalocean/digitalocean2.tmpl

which are different to what they are in template 1 and I can do this as follows:

>     export APPLICATION="joomla" #MANDATORY (joomla or wordpress or drupal or moodle)
>     export BASELINE_DB_REPOSITORY="communitybuilder-db-baseline" #MANDATORY
>     export APPLICATION_BASELINE_SOURCECODE_REPOSITORY="communitybuilder-webroot-sourcecode-baseline" #MANDATORY

You can make any other adjustments you want like if you want to choose APACHE instead of NGINX or change the size of the machines (you can find out about such things in the specification).

With your baseline template fully configured, you are now ready to perform a baseline build (in other words, directly install a live application starting from zilch).

If your template is configured correctly you can now run the build process selecting the appropriate template and cloudhost (digitalocean and template 2 in other words). On your build machine, do as follows:

>     cd ${BUILD_HOME}
>     ./ExpeditedAgileDeploymentToolkit.sh

and answer any questions (selecting template 2 this time when prompted) and have a bit of patience whilst the build runs. 

When I ran my baseline build the output I got from the ADT was as follows:  
![](images/expedited/do-43.png "Digital Ocean Tutorial Image 43")
![](images/expedited/do-44.png "Digital Ocean Tutorial Image 44")
![](images/expedited/do-45.png "Digital Ocean Tutorial Image 45")
![](images/expedited/do-46.png "Digital Ocean Tutorial Image 46")
![](images/expedited/do-47.png "Digital Ocean Tutorial Image 47")
![](images/expedited/do-48.png "Digital Ocean Tutorial Image 48")
![](images/expedited/do-49.png "Digital Ocean Tutorial Image 49")

If you follow these steps, then, you will have a copy of your community builder customised Joomla application running on digitalocean.

![](images/expedited/do-67.png "Digital Ocean Tutorial Image 67")

**Leave the servers you have deployed running for use in the next tutorial in the series.**

[Temporal CMS Installation](./expedited-temporal-joomla.md)
