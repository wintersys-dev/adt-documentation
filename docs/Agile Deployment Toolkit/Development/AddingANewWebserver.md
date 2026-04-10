To add a new webserver type (for example, litespeed or caddy) you will need to add or modify the following files:

>     adt-autoscaler-scripts/autoscaler/InitialiseCloudInit.sh
>     adt-autoscaler-scripts/services/server/cloud-init/*

>     adt-build-machine-scripts/initialisation/InitialiseCloudInit.sh
>     adt-build-machine-scripts/services/server/cloud-init/*
>     adt-build-machine-scripts/templatedconfigurations/quick_specification.dat
>     adt-build-machine-scripts/templatedconfigurations/specification.md

>     adt-webserver-scripts/installation/InstallAuthenticator.sh
>     adt-webserver-scripts/installation/InstallNGINX.sh
>     adt-webserver-scripts/installation/InstallPHPBase.sh
>     adt-webserver-scripts/installation/InstallWebserver.sh
>     adt-webserver-scripts/installation/nginx/BuildNginxFromSource.sh
>     adt-webserver-scripts/services/dns/TrustRemoteProxy.sh
>     adt-webserver-scripts/services/webserver/*
