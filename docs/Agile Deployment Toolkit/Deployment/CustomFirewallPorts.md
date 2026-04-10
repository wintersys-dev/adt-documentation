#### ADDING CUSTOM PORTS TO THE BUILD CONFIGURATION

The Agile Deployment Toolkit takes care of the standard ports that need to be open for each of your machines. 
Generally this will involve the SSH port that you are using, the 443 port for https to the webserver, limited
access to the port that your database is running on and the ability to communicate through the firewalls for
any local VPC communication (on the providers that firewall inter-VPC traffic). 
However it is possible that you might either want to set custom ports to be open for a particular deployment
if you have some additional software which is not in the core toolkit which requires such as if you have installed
phpmyadmin you  might want port 8080 to be accessible from your laptop IP address on the webserver machine. 
And so to that end I try to make it easy by facilitating custom ports. The file where you need to set your
custom ports is located at 

>     ${BUILD_HOME}/descriptors/customfirewall.dat

and if you wanted port 8080 to be open to your laptop IP address (in other words only your laptop IP address can 
connect to port 8080 on your webserver then you would configure this file as follows if your laptop IP address is
85.14.120.84

>     AUTHENTICATORCUSTOMPORTS:  
>     REVERSEPROXYCUSTOMPORTS:  
>     AUTOSCALERCUSTOMPORTS:  
>     WEBSERVERCUSTOMPORTS:8080|ipv4|85.14.120.84/32   
>     DATABASECUSTOMPORTS:  

As you can see there are no other custom ports for other machine types. 

If you wanted to configure port 8080 so it can be accessed from any machine on the Internet then you would configure
the webserver relevant line as:

>     WEBSERVERCUSTOMPORTS:8080|ipv4|0.0.0.0/0  

When you run the build process your if the firewall native to your provider is configured to be active then this port
will be open as specified in your native firewall for your webserver machine and if the OS firewall is active (either
ufw or iptables) then the OS firewall will be configured to grant access through that port to the ip address you have
specified.

-----------------------------------

#### DISABLING CUSTOM PORTS AFTER THE BUILD IS DEPLOYED

So, to be as secure as possible the example I have given phpmyadmin you want it to be completely firewalled off when 
you are not directly accessing it and so to do that is a two step process. Once the servers are live each server has
the custom ports file located at 

>     ${HOME}/runtime/customfirewall.dat

And so the two steps that you need to do to raise the firewall completely when you are not using a particular custom port
are:

**STEP 1**

Edit the customfirewall.dat file on the appropriate machine type and if you want to firewall off port 8080 again
on the current machine, edit the file to look like this:

>     AUTHENTICATORCUSTOMPORTS:  
>     REVERSEPROXYCUSTOMPORTS:  
>     AUTOSCALERCUSTOMPORTS:  
>     WEBSERVERCUSTOMPORTS:8080|ipv4|85.14.120.84/32|DELETE  
>     DATABASECUSTOMPORTS:  

What this will then do is revoke the access rights to that IP address to port 8080 by raising the firewall again. This will
ONLY AFFECT the OS firewall it will not affect the provider native firewall. 

**STEP 2**

If the native firewall is active go to your provider native firewall and remove the appropriate record for your provider native firewall
(it is assumed that if you are reading this you know how to do that).

--------------------

#### ENABLING/RE-ENABLING CUSTOM PORTS AFTER THE BUILD IS DEPLOYED

If a couple of days have passed and you need to renable port 8080 for the phpmyadmin that you have modified the toolkit to make use of

**STEP 1**

Go to your provider native firewall and allow your IP address through the firewall for your desired port, for example, 85.14.120.84/32

**STEP 2**

Edit the customfirewall.dat file on the appropriate machine type and if you want to allow port 8080 through the OS firewall again
on the current machine, edit the file to look like this:

>     AUTHENTICATORCUSTOMPORTS:  
>     REVERSEPROXYCUSTOMPORTS:  
>     AUTOSCALERCUSTOMPORTS:  
>     WEBSERVERCUSTOMPORTS:8080|ipv4|85.14.120.84/32  
>     DATABASECUSTOMPORTS:  

If you want to add allow an additional port to be open on the webserver machine and also your database machine lets say port 5143 for all IPV4 
IP addresses then your configuration file would look like:

>     AUTHENTICATORCUSTOMPORTS:  
>     REVERSEPROXYCUSTOMPORTS:  
>     AUTOSCALERCUSTOMPORTS:  
>     WEBSERVERCUSTOMPORTS:8080|ipv4|85.14.120.84/32 5142|ipv4|0.0.0.0/0  
>     DATABASECUSTOMPORTS:5142|ipv4|0.0.0.0/0  

I think that's the basic usecases for custom firewall ports as it is currently implemented. Whatever your requirements are it should be some 
combination of these steps. 



