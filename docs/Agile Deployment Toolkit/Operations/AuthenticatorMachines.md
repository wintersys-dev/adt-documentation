#### Authenticator Machine Operational Processes

##### Firewall based approach

1. The user will go to the authentication webserver and enter the email address that they have been provided with by your administrators. For example, if your domain email is nuocial@uk then the user might enter bob@nuocial.uk in order to receive an authentication email

2. If the user has entered a valid email address that user will receive a unique link in their inbox that will provide a temporary unique link to a file on your authentiction server. When they click that link they will be prompted to enter the ip address of their laptop

3. They enter their laptop IP address and it is appended (along with any other ipaddresses from other users that have been activated concurrently) to the file 

>     /var/www/firewall/ipaddresses.dat

So, what we have now is a list of ip addresses that have requested access to our systems as a regular user and so we need to now process those ip addresses and grant them the requiste access to our webservers. 

Processing of incoming IP addresses for firewall based access

1. The incoming ip addresses are checked for validity and if they are a valid IP address then they are written to a file 

>     ${HOME}/runtime/authenticator/ipaddresses.dat.${machine_ip}

where the machine ip address is the ip address of the current authenticator

2. Once all the ip addresses have been processed, the file containing above is written to the datastore under tag firewall-auth-laptop-ips. What that means is that any point in time n authenticators are all writing the list of ip addresses that they have accepted for the current iterationn to the datastore distinguised by the ip address of the authenticator itself so that they don't overwrite each others updates because if there was a single file with the ip addresses there would be a race condition in regards to which authenticator gets its updates persisted. I use sleep periods to try and ensure that when multiple authenticators don't update the datastore concurrently but, by using the IP addresses to distinguish the updates makes absolutely sure there is no contention.

3. A period of time later every reverse proxy machine in our infrastructure will obtain the ipaddresses.dat.${machine_ip} files from the firewall-auth-laptop-ips. This is done by the script:

>     ${HOME}/services/security/firewall/AllowAuthenticatorIPAddress.sh

And on each reverse proxy this script will allow access to the port 443 from the ipaddresses that have been obtained from the authenticator machine. Access is granted depending on which firewall solution is active, iptables or ufw. Once the ipaddress is allowed through the firewall, the user should be able to access the reverse proxy machines (and therefore your webproperty) from their laptop. 

NOTE1: The reverse proxy firewall provided by your VPS provider (if active) needs to allow more open access to port 443, in other words it allows through all IP addresses and its only the OS based firewall which limits access to individual IP addresses. 

NOTE2: If you are using this technique you need tech savvy users because if their IP address changes, for example, they registered their home laptop wifi IP address and then they are on the bus and they try to login with with their mobile phone using their mobile data their ip address will be different and they will need to know that the timeout means that they need to visit the authentication machine and register their new ip address in order to gain access.

NOTE3: If you take your infrastructure offline and redeploy it then all accepted ip addresses will be reset meaning that all the users of the system will get timeouts and they will need to know that a timeout without any explanation means that their IP address isn't recognised and they need to register it through the authentictors (which is only a couple of minutes to do) to regain access.
