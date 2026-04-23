To add a new DNS provider you will need to review and add to the following files for your new DNS provider. It would be cool to have other providers supported that provide services similar to cloudflare to provide deployers with other options

>     adt-autoscaler-scripts/autoscaler/AddIPToDNS.sh
>     adt-autoscaler-scripts/autoscaler/GetDNSIPs.sh
>     adt-autoscaler-scripts/autoscaler/RemoveIPFromDNS.sh

>     adt-build-machine-scripts/buildservers/FinaliseBuildProcessing.sh
>     adt-build-machine-scripts/initialisation/InitialiseDNSRecord.sh
>     adt-build-machine-scripts/services/dns/*
>     adt-build-machine-scripts/services/security/firewall/GetProxyDNSIPs.sh
>     adt-build-machine-scripts/services/server/ObtainSSLCertificate.sh
>     adt-build-machine-scripts/templatedconfigurations/ValidateTemplate.sh
>     adt-build-machine-scripts/specifications/quick_specification.dat
>     adt-build-machine-scripts/specifications/specification.md

>     adt-webserver-scripts/services/webserver/configuration/InstallNginxConfigurationForAuthenticator.sh
>     adt-webserver-scripts/services/webserver/configuration/InstallNginxConfigurationFromRepo.sh
>     adt-webserver-scripts/services/webserver/configuration/InstallNginxConfigurationFromSource.sh
>     adt-webserver-scripts/security/ObtainSSLCertificate.sh
>     adt-webserver-scripts/security/SetupFirewall.sh
