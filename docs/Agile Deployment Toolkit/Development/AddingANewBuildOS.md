From time to time new versions of the ubuntu and debian operating systems are released. To integrate a new version (for example ubuntu 26.04 or debian 13)  
the following files have to be updasted to support these new versions:
 
>     adt-autoscaler-scripts/services/server/GetOperatingSystemVersion.sh

>     adt-build-machine-scripts/services/server/GetOperatingSystemVersion.sh
>     adt-build-machine-scripts/templatedconfigurations/ValidateTemplate.sh
>     adt-build-machine-scripts/templatedconfigurations/quick_specification.dat
>     adt-build-machine-scripts/templatedconfigurations/specification.md

>     adt-webserver-scripts/installation/InstallPHPBase.sh

You can look at the examples of other versions of each OS to see how the updates need to be made. 

I am not sure how possible this is but if you wanted to have a go at deploying to, for example, rocky linux you would need to change the following  files:

>     adt-autoscaler-scripts/installation/*
>     adt-autoscaler-scripts/services/cloudhost/*
>     adt-autoscaler-scripts/utilities/processing/RunServiceCommand.sh
>     adt-autoscaler-scripts/utilities/software/UpdateSoftware.sh

>     adt-build-machine-scripts/ExpeditedAgileDeploymentToolkit.sh
>     adt-build-machine-scripts/helpers/RunServiceCommand.sh
>     adt-build-machine-scripts/initialisation/InitialiseCompatibilityChecks.sh
>     adt-build-machine-scripts/initialisation/InitialiseCompatibilityChecks.sh
>     adt-build-machine-scripts/installation/*
>     adt-build-machine-scripts/services/cloudhost/*
>     adt-build-machine-scripts/templatedconfigurations/ValidateTemplate.sh
>     adt-build-machine-scripts/templatedconfigurations/quick_specification.dat
>     adt-build-machine-scripts/templatedconfigurations/specification.md
>     adt-build-machine-scripts/templatedconfigurations/templateoverrides/OverrideScript.sh

>     adt-database-scripts/installation/*
>     adt-database-scripts/utilities/processing/RunServiceCommand.sh
>     adt-database-scripts/utilities/software/UpdateSoftware.sh

>     adt-webserver-scripts/installation/*
>     adt-webserver-scripts/utilities/processing/RunServiceCommand.sh
>     adt-webserver-scripts/utilities/software/UpdateSoftware.sh
