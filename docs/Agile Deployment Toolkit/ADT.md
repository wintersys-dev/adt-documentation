# Welcome to the Agile Deployment Toolkit Documentation  

**AN INTRODUCTION TO THE ADT**  

The Agile Deployment Toolkit is a power tool for CMS deployments on VPS machines. In the hands of a skilled person there are advantages to this approach. In summary:

**Is c-panel not ideal for your web hosting because you want more server control?**

**Do you want to have your own set of configured VPS servers but do not want to spend the time configuring and deploying software on them?**

**Do you want to have consistent security processes baked into your server deployments?**

**Would you like to be able to redeploy to a different (supported) VPS hosting provider easily - within 5 - 10 minutes if needed?**

**Would you like the ability to be able to extend the toolkit to be able to deploy to any compatible (for example AWS or Google Cloud) VPS provider according to your preference?**

**Would you like the ability to be able to extend the toolkit to be able to extend applications for your own needs such as NextCloud or Grav?**

**Does automated server configuration with the latest releases of software included for each deployment sound attractive?**

**Would you like a low barrier to entry solution using nothing but basic scripting techniques?**

**How do you like having total control of your servers without being beholdent to the configuration whims or limitations of a 3rd party hosting provider?**

**Would you like the ability to add in any customisations that you choose to your hosting solution and not be limited by services offerings provided by a particular third party?**  

Then you might be interested in the Agile Deployment Toolkit which currently supports Joomla, Wordpress, Drupal and Moodle CMS applications and Digital Ocean, Exoscale, Linode and Vultr VPS hosting providers.

------------

**OBJECTIVE**

The intended objective for developing this toolkit is to be able to deploy large scale social systems consistently, repeatedly and resiliently. These will likely be systems with dynamic data and generally not just static blogs for which other solutions might suit better. For systems with dynamic user interation, the extra horsepower of multiple webserver machines can be useful at scale.   

Most of the effort that I have put into the development of the ADT relates to design and strucuture. In other words, you won't find the most highly optimised nginx or apache configurations shipped with this toolkit but that is basically the point. You can plug and play your own (highly optimised) configurations for webservers easily because of the design decisions that I made.  

-------------

**STATUS**

At the time of writing (April 2025) I am just about to go through final testing to make things as resilent as I can as a lone developer and after that what I have built will need to be put through its paces by the community to eventually get to the point where my software is considered stable, tested and production ready. There's a lot of different configurations possible and so the more it is used the more sure that all configurations are fully resilient. I have valiantly put everything through as much testing as I could muster, but, it's orders of magnitude easier when the community puts it through its paces also; you could say, 'stronger together than individual'. 

---------------

**SOURCECODE REPOSITORIES**

There are four repositories associated with this toolkit: 

**[Agile Infrastructure Build Machine Scripts](https://github.com/wintersys-dev/adt-build-machine-scripts)**   
**[Agile Infrastructure Autoscaler Scripts](https://github.com/wintersys-dev/adt-autoscaler-scripts)**   
**[Agile Infrastructure Webserver Scripts](https://github.com/wintersys-dev/adt-webserver-scripts)**  
**[Agile Infrastructure Database Scripts](https://github.com/wintersys-dev/adt-database-scripts)**  

NOTE: The scripts in these repositories will control the build process for the three different classes of server machines (autoscaler/webserver/database server). You could potentially run the build process on a dedicated linux (Ubuntu/Debian) laptop that you boot off a portable linux flash drive (with persistent storage enabled). I generally describe how to use this toolkit from a VPS linux machine and you SHOULD NOT use your day to day linux laptop as your build machine because the build process will install software that you might not want on your daily laptop and will also make some configuration changes to your system.

----------------------

**ESSENTIAL DOCUMENTATION**

The Quick start demos below are specifically designed to give you a taste for this with the least effort possible and if you like what you see, you can do a deeper dive into what I have built here. 

**[Quick Start Demos](<Demos/QuickStartDemos.md>)**  
**[Deployment Tutorials](<Tutorials/TutorialsMenu.md>)**  
**[The Specification](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/specifications/specification.md)**


----------------------------------

**WHAT IS A DMS(Deployment Management System)**

This is a DMS system, so, what is a DMS system? A DMS is for helping with deploying servers using well tested and secured processes. Its power is that it can be extended easily and is very similar in design character to a CMS system but for deployment rather than content. The ultimate objective is to make it possible to have your own bespoke application libraries. For example, in Joomla, something like a JAD (Joomla Applications Directory), in Wordpress something like a WAD (Wordpress Applications Directory) and in Drupal something like a DAD (Drupal Applications Directory). What this template repo gives you is the core structure which plugs into the ADT around which to build out your custom server type. 

You can learn more about application directories **[here](<Development/ApplicationDirectories.md>)**
 
------------------------

**THE CORE**

With the core of the Agile Deployment Toolkit, it will make use of a set of services and providers. I elected to use Digital Ocean, Exoscale, Linode and Vultr to deploy on, or, as deployment options, but, the toolkit is designed to be forked and extended to support other providers maybe AWS, Rackspace, Google Cloud and so on. 
In ordinary usage, you should work from your own forks of the repositories because you will likely want to change the configurations of APACHE, NGINX, or LIGHTTPD or other products with configuration profiles within the sourcefiles of your forked repository and you can't do that if you deploy off the this main core set "wintersys" provided repositories. 

The full set of services that are supported by the core of the toolkit and which you can extend in your forks is:

1. For VPS services, one of - **Digital Ocean**, **Linode**, **Exoscale** or **Vultr**
2. For Email services, one of - **Amazon SES**, **Mailjet** or **Sendpulse**
3. For Git based services, one of - **Bitbucket**, **Github** or **Gitlab**
4. For DNS services, one of - **Cloudflare DNS**, **Digital Ocean DNS**, **Exoscale DNS**, **Linode DNS**, **Vultr DNS** 
5. For S3 compatible object store services, one of - **Digital Ocean Spaces**, **Exoscale Object Store**, **Linode Object Store**, **Vultr Object Store**
6. I chose these VPS providers to deploy with because they have managed database offerings, if you wish to make a production ready deployment with this toolkit it is recommended that you make your deployment using their managed database offerings. For development deployments, you can use the default "apt" database install to your own VPS system within your VPC that this toolkit provides. 

--------------------------------

**BUILD METHODS OVERVIEW**

There are two types of build method you can employ to get a functioning application. There is the **hardcore** build (only use once you are more experienced with this tool), and the **expedited** build method. 

-------------------------------

**THE CONCLUSION**

A DMS system such as this one can certainly speed your server deployments. It does require skill, and therefore learning to use but once you get a taste for it the result can be sweet.  

