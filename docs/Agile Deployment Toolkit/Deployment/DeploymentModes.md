#### TOOLKIT DEPLOYMENT MODES

There are two deployment modes depending upon what you want to be doing. There is development mode for when you are developing an application and there is production mode for when you want a live application with users and visitors. 

To set development mode to the active mode, you need to set the following template value:  

>     DEPLOYMENT_MODE="DEVELOPMENT"

To set production mode to the active mode you need to set the following template value:  

>     DEPLOYMENT_MODE="PRODUCTIOM"


##### DEVELOPMENT MODE

I have never used a localhost development environment to build a website using a CMS so I don't know what features they have but I presume that if its running on a localhost that its only accessible locally, the idea being to develop locally and then publish to the web once your development is complete.
This most probably works OK in a lot of situations but, if you want to have a multi-developer approach to developing a web property with a CMS then its most likely going to go easier on you if you develop to on a server that is internet accessible. Quite possibly you have other ways to make that possible but the way I do it is to simply have a development mode where only a webserver machine and a database machine are deployed that the CMS application developers can access from anywhere, firewall permitting. This way you can have multiple developers all working on the production of a single web property. The way this would work is that development would take place against an limited function "internet accessible setup" and then once the development is complete the application can be deployed to production where additional functionality is available such as autoscaling reverse proxies and so on. 

##### PRODUCTION MODE

Production mode can be used to deploy autoscaling - it is static autoscaling at the moment rather than dynamically adjusting scaling based on load because its expected that the type of websites developed will have predictable traffic and therefore won't need to scale up or scale down in response to some viral event. It might be an interesting enhancement to make to add in a "dynamic scaling" feature so that machines are provisioned in short order automatically if there is a load spike. Production mode can also run webservers across multiple regions (of the same or different providers) whilst using a managed database from one of the providers. There is obviously a performance consideration but this can be kept to a minimum if your regions are relatively close such as Amsterdam, London and Frankfurt rather than London, Melbourne and San Francisco. The multi region deployments are quite involved from a design point of view and so to make a mental note here that any feedback on whether the multi-region deployment style is well functioning or not in a live situation would be valuable. At the time of writing this is a skunkworks project by one developer so I am as interested as anyone in how well it works under real world usage. 


