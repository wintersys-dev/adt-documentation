SERVER CONFIGURATION SETTINGS

On each of the servers is a file which contains all the live configuration settings for that server. These settings are obtained from the values set in the template and are passed to the correct server type as part of the "cloud-init" process of each server that is provisioned. Note, this still has to be taken care of when an autoscaler is provisioning a webserver - in other words, the autoscaler has to be a store of the configuration settings that newly provisioned webservers will be configured with.

>     On the autoscaler it is called ${HOME}/runtime/autoscaler_configuration_settings.dat  
>     On the webserver it is called ${HOME}/runtime/webserver_configuration_settings.dat 
>     On the database it is called ${HOME}/runtime/database_configuration_settings.dat
>     On the reverseproxy it is called ${HOME}/runtime/reverseproxy_configuration_settings.dat
>     On the authenticator it is called ${HOME}/runtime/authenticator_configuration_settings.dat

Once the servers are running these files are consulted regularly during normal operation to find out, for example, which webserver type we are running or credentials of various kinds when services are being accessed and so on. You can change the values in this files, if, for example an access token for a service you use (lets say an SMTP email service) expired you can update these files with refreshed values by visting each active server in turn (including the webserver configuration file on the autoscaler) and updating the appropriate value(s) and the system would then use your updated values.   

You have to know what you are doing, but, I am just showing you here that it is possible to change the operation after deployment. You need to reference the [spec](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/master/specifications/specification.md) to find out what each variable in these files is for. 

