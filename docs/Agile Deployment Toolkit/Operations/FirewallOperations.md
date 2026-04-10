if **ACTIVE_FIREWALLS = 0** - No active firewalls  
if **ACTIVE_FIREWALLS = 1** - UFW or iptables firewall only active on all machines   
if **ACTIVE_FIREWALLS = 2** - Native firewall only active on all machines  
if **ACTIVE_FIREWALLS = 3** - UFW or iptables and Native Firewall active on all machines  

On each machine there are two core scripts related to the firewalling of the machine

>      ${HOME}/security/KnickersUp.sh
>      ${HOME}/security/SetupFirewall.sh

Knickers up allows all outgoing connections and denies all incoming connections and this is the base position for the firewall

The script SetupFirewall.sh will selectively allow certain IP addresses to connect to certain ports. For example, the build machine is allowed to connect to the machines through the SSH port and if the machine is a webserver then the depending on the configuration, client ip addresses are allowed to connect to the machine through the 443 port also, if you you are configured to use cloudflare, only cloudflare IP addresses are allowed to connect to port 443 giving you some extra protection and if you are using an authentication server all client ip addresses are firewalled from access to port 443 of the webserver until they have been authenticated as valid by the authentication server you are running. 

The SetupFirewall script is run from cron on a minute by minute basis

If the firewall were to be inactive for some reason there is a script

>      ${HOME}/security/MonitorFirewall.sh

Which runs every minute and if the firewall were to become inactive an email will be sent and attempts made to restore the firewall to an active condition or state

If you have other configurations of firewalling that you need for any additional applications you install, then, you can modify the "SetupFirewall" script on each machine type so that the firewalling that you have meets your needs. 

When **ACTIVE_FIREWALLS = 2** or **ACTIVE_FIREWALLS = 3** then the native firewall is configured on each build. This takes place in the script

>     ${BUILD_HOME}/services/security/firewall/SetupNativeFirewall.sh

on the build machine. This script will set up a native firewall for each machine type. 

>     adt-autoscaler
>     adt-webserver
>     adt-database

As well as adding machines of each requisite type to the necessary firewall. On the autoscaler when a new webserver is provisioned the newly created machine is added to the firewall as it is provisioned/created using the CLI

Each firewall type will allow connections from its local VPC for SSH and database connections as well as any necessary public connections if (for example) a build machine is deployed outside the VPC or if port 443 on a webserver needs to be accessed from ourside the VPC. This way, the firewall is kept as tight as possible. And so if ACTIVE_FIREWALLS="3" you have double firewall protection, UFW of iptables as well as the native firewalling system provided by your VPS provider. 

Note, the build-machine is not added to any native firewall. When you  first provision your build-machine if you want to have a native firewall as well, you need to add the build-machine manually to the native firewall that you have set up. All other machine types are automatically added to the native firewalling system when appropriate. 

Notice in the [Override Script](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/templatedconfigurations/templateoverrides/OverrideScript.sh) that you are asked to provide your laptop IP address.
This is so that the firewall can be setup to protect your newly provisioned build-machine.
If you look at the Override Script in detail you will find the command:

>     ${BUILD_HOME}/services/security/firewall/InitialiseFirewall.sh 

At the end of the script. This is the call which will setup your firewall (ufw or iptables) to only allow your laptop IP to connect to it and only to the SSH_PORT that you have configured.
This makes sure we are doing our best to stay tight. 
If you want to add an extra layer of protection in case anything happens to this firewall by having a native firewall setup which you can do through the gui system for the VPS provider you are using and connecting your build-machine to it only allowing through your laptop IP address.

This is only a first step in the lifecycle of your build machine. Over the course of time your laptop ip address may change or you may want to grant access to a trusted team mate and so only having your build machine accept connections from 1 ip address wouldn't do. So, you can follow:

[AdjustBuildMachineFirewall](../Deployment/AdjustBuildMachineAccess.md)

To understand the practical steps that you need to go through to allow access to other ip addresses that you trust. The process described is activated by a cron task on the build-machine which calls the script

>     ${BUILD_HOME}/services/security/firewall/AdjustBuildMachineFirewall.sh

Which updates the firewall of the build machine for any new ip address that you need to allow access from. With the native firewalling option on the build-machine you will need to update the native firewall to allow access to your friend's ip address manually. 

My advice is if you want the easiest option is once you have allowed your friend's ip address access to your build machine to "switch access on and off" using the gui system of the native firewall. Is that clear. 

If your friend's IP address is 111.111.111.111 and you have allowed that IP address into your system by following:

[AdjustBuildMachineFirewall](../Deployment/AdjustBuildMachineAccess.md)

Then also add that IP address to the firewalling system that your cloudhost provides and control whether your friend has access or not using the native firewalling system GUI rather than the full "AdjustBuildMachineFirewall.sh" process. Is that clear how that is an easier way. In other words, you  only use the AdjustBuildMachineFirewall.sh route for initial access to an IP address after that you then control access by allowing or denying in the GUI system of your VPS provider.
