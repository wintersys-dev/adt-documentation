To support another cloudhost provider (for example, AWS, google cloud, OVH cloud and so on) you should be able to get it going by modfying the following files:


>     adt-autoscaler-scripts/installation/InstallCloudhostTools.sh
>     adt-autoscaler-scripts/installation/InstallPackageManager.sh
>     adt-autoscaler-scripts/services/server/GetOperatingSystemVersion.sh
>     adt-autoscaler-scripts/services/cloudhost/InitialiseCloudhostConfig.sh
>     adt-autoscaler-scripts/services/dns/*
>     adt-autoscaler-scripts/services/dbaas/AdjustDBaaSFirewall.sh
>     adt-autoscaler-scripts/services/server/*
>     adt-autoscaler-scripts/security/SetupFirewall.sh

>     adt-build-machine-scripts/ExpeditedAgileDeploymentToolkit.sh
>     adt-build-machine-scripts/Log.sh
>     adt-build-machine-scripts/helpers/*
>     adt-build-machine-scripts/initialisation/InitialiseCloudhostConfig.sh
>     adt-build-machine-scripts/initialisation/InitialiseDatabaseService.sh
>     adt-build-machine-scripts/installation/InstallCloudhostTools.sh
>     adt-build-machine-scripts/services/server/GetOperatingSystemVersion.sh
>     adt-build-machine-scripts/services/dns/*
>     adt-build-machine-scripts/services/security/*
>     adt-build-machine-scripts/services/server/*
>     adt-build-machine-scripts/selection/SelectCloudhost.sh
>     adt-build-machine-scripts/templatedconfigurations/OverrideTemplate.sh
>     adt-build-machine-scripts/templatedconfigurations/ValidateTemplate.sh
>     adt-build-machine-scripts/templatedconfigurations/quick_specification.dat
>     adt-build-machine-scripts/templatedconfigurations/specification.md
>     adt-build-machine-scripts/services/security/ssl/acme/acme-overrides

>     adt-database-scripts/application/db/maria/InstallApplicationDB.sh
>     adt-database-scripts/installation/InstallPackageManager.sh

>     adt-webserver-scripts/installation/InstallPackageManager.sh
>     adt-webserver-scripts/security/ObtainSSLCertificate.sh
>     adt-webserver-scripts/security/SetupFirewall.sh


If you want to add a new cloudhost to your the toolkit you will need to configure a template with placeholders for your new cloudhost's CLI tool  
You can find examples here:  

>     ${BUILD_HOME}/initialisation/configfiles

This script wil then initialise your config file template by replacing placeholder values with live values

>     ${BUILD_HOME}/initialisation/InitialiseCloudhostConfig.sh


Its the same process on the autoscaler where you put your template in

>      ${HOME}/services/cloudhost/configfiles

and the script below will swap out the placeholders you have set for live values:

>     ${HOME}/services/cloudhost/InitialiseCloudhostConfig.sh

NOTE: originally the core supported AWS but I found I had to make various AWS specific customisations so I stripped AWS out of the core to keep the core as simple and consistent as possible. If you want to put the work in to add support for AWS, then, you might get some clues from my archived repositories which you can find below:  

[build-machine-with-aws](https://github.com/wintersys-dev/adt-build-machine-scripts-withaws)  
[autoscaler-with-aws](https://github.com/wintersys-dev/adt-autoscaler-scripts-withaws)  
[webserver-with-aws](https://github.com/wintersys-dev/adt-webserver-scripts-withaws)  
[database-with-aws](https://github.com/wintersys-dev/adt-database-scripts-withaws)  
