AUTHENTICATION SERVER WORKFLOW

Firewall technique

When a user inputs their email address to the authentication server, behind the scenes the following process is taking  place:

>     website_url="https://${WEBSITE_URL}/ip-address-${file_name}.html"
>     message="<!DOCTYPE html> <html> <body> <h1>IP address authorisation form for ${WEBSITE_URL_ORIGINAL}</h1> <p>From the SAME browser as you want to connect from (your phone broswer might have a different ip address to your laptop if one is on WIFI and one is on 5G go to www.whatsmyip.com and enter the IPV4 IP address in the form that appears when you click the link below. Cheers. This link will be valid for 5 minutes before being deleted. </p> <a href='"${website_url}"'>Enable Your IP Address</a> </body> </html>"
>     ${HOME}/services/email/SendEmail.sh "Authenticated IP claim request for ${WEBSITE_URL_ORIGINAL}" "${message}" MANDATORY ${email_address} "HTML" "AUTHENTICATION"

The user will then receive an email with a link in it to enter their laptop IP address to. When the user inputs their IP address then the following process will take place:

>     ${HOME}/services/datastore/operations/MountDatastore.sh "firewall-auth-laptop-ips" "distributed" 
>     ${HOME}/services/datastore/operations/PutToDatastore.sh "firewall-auth-laptop-ips" ${HOME}/runtime/authenticator/ipaddresses.dat.${machine_ip} "firewall-laptop-ips" "distributed" "no"

On your reverse proxy machines the crontab will call:

>     */1 * * * * export HOME="${HOME}" && ${HOME}/security/AllowAuthenticatorIPAddress.sh

And the reverse proxy will allow the supplied IP address through the firewall

for ufw

>     /usr/sbin/ipset add allowed-laptop-ips "${ip_address}/32"     

for iptables

>     /usr/sbin/ipset add allowed-laptop-ips ${ip_address}

------------------------------------

Basic-auth technique

When a user inputs their email address to the authentication server, behind the scenes the following process is taking  place:

A new username and password are created related to the input from your user:

>     /usr/bin/htpasswd -b -c ${basic_auth_file} ${username} ${password}

Once the username and password is generated it is emailed to the user

>     message="<!DOCTYPE html> <html> <body> <h1>The basic auth password you requested for ${WEBSITE_URL} is: ${password} </body> </html>"
>     ${HOME}/services/email/SendEmail.sh "Basic Auth password request" "${message}" MANDATORY ${username} "HTML" "AUTHENTICATION"


It is also written to a common object storage bucket such as:

>     ${HOME}/services/datastore/operations/MountDatastore.sh "basic-auth-credentials" "distributed" 
>     ${HOME}/services/datastore/operations/PutToDatastore.sh "basic-auth-credentials" ${basic_auth_file}.${machine_ip} "basic-auth-credentials" "distributed" "no"

On your reverse proxy machines the crontab will call:

>     */1 * * * * export HOME="${HOME}" && ${HOME}/security/ObtainBasicAuthCredentials.sh


and within the cron script a call will be made to obtain the basic auth credentials and make them available for use on the current reverse proxy:

>     ${HOME}/services/datastore/operations/GetFromDatastore.sh "multi-region" "multi-region-basic-auth/*" "${HOME}/runtime/authenticator/incoming"
