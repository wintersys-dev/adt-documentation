### Adding a new application to the toolkit

The core supports joomla,wordpress,drupal and moodle. If you want to support further applications such as "NextCloud" and a whole host of other potential applications I believe you should be able to integrate support for any (php) application which follows a similar pattern by updating and modifying files in these directories or in the case of specific files particular files themselves.

>     adt-autoscaler-scripts/autoscaler/SelectHeadFile.sh

>     adt-build-machine-scripts/processing/*
>     adt-build-machine-scripts/services/application/*
>     adt-build-machine-scripts/templatedconfigurations/ValidateTemplate.sh
>     adt-build-machine-scripts/templatedconfigurations/quick_specification.dat
>     adt-build-machine-scripts/templatedconfigurations/specification.md

>     adt-webserver-scripts/application/configuration/*
>     adt-webserver-scripts/application/monitoring/CheckServerAlive.sh
>     adt-webserver-scripts/application/processing/*

The most major part of the integration is likely to be adt-webserver-scripts/services/webserver/configuration and will most likely take the majority of your effort
