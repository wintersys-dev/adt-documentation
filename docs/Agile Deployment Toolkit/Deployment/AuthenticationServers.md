#### Use cases for authentication servers:  

This toolkit can be used for anonymous access public websites like blogging websites etc, but, primarily it is designed for use with authenticated or authorised users. In particular it is designed so that as an owner of a web property such as a social network you are running you assign your members a bespoke email address that is associated with access to your web property. In other words anyone that is granted one of your email addresses is considered to have been vetted as not being a malign influence and so you can use your domain specific email addresses to gate access to your web property and essentially that is what authenticator machines are designed to be in your infrastructure configuration either in a firewall gated way, a whitelist gated way, a basic auth gated way or a wire guard gated way. Below are some more details on how I have designed this to work in each case. Please be aware that it's very likely that you will want to task someone in your team (or possibly purchase a solution from a trusted third party) that is responsible for running your domain specific email server. The bottom line is that with authenticator machines you need to control the DNS records associated with your own domain email address and (for security reasons because authentication information shouldn't be sent via an email system that you don't yourself control) you own custom email server deployment.

You can check [here](./RollYourOwnMailserver.md) for my thoughts about which email server solutions might fit your needs well.

You can deploy an authenticator machine to your current region if you want to require your users to go through initial authentication before they access your web property. Authentication can take place using the following methods. 

To use an authentication server to control access you also need to have one or several reverse proxy machines running. In other words, you can't run authenticators unless you are also running reverse proxies.

NOTE: the firewall is not touched on the webserver(s) by this method the firewall access is controlled through the reverse proxy machines and so you have to be using reverse proxies for these authentication methods to be possible. 

##### 1. Firewall based authentication.

To use firewall based authentication you will need to be using reverse proxy(ies) in front of your webserver(s) and you will need to deny access by default to your reverse proxies by setting an appropriate value in firewall.dat, for example:


>     AUTHENTICATORPORTS:cloudflare|ipv4|cloudflare  
>     REVERSEPROXYPORTS:
>     AUTOSCALERPORTS:
>     WEBSERVERPORTS:
>     DATABASEPORTS:

To use this method your users will have to have a certain level of sophistication and understanding but you could make it clear in a simple statement. 

"If you experience any timeouts please tell us your current IP address by going to the authentication server to regain access"

NOTE: the firewall is not touched on the webserver(s) by this method the firewall access is controlled through the reverse proxy machines and so you have to be using reverse proxies for this method to be possible. 

In your template to set up an authentication server if your webproperty is something like nuocial.uk and you are putting cloudflare in front of your authentication server you will need settings for firewall based authentication as something similar to:

>     ######Authentication Server#####
>     export NO_AUTHENTICATORS="1"
>     export AUTHENTICATOR_TYPE="firewall"
>     export AUTH_SERVER_URL="auth.nuocialsecurity.uk"
>     export AUTH_DNS_USERNAME="webmaster@nuocial.uk" (or whatever the email address for your cloudflare account is)  
>     export AUTH_DNS_SECURITY_KEY="X1234X"   (your cloudflare API key)
>     export AUTH_DNS_CHOICE="cloudflare"
>     export USER_EMAIL_DOMAIN="nuocial.uk" (the custom domain that you have issued email addresses for, for example, user1@nuocial.uk)

-----------------------------------

-----------------------------------

##### 2. Basic auth based authentication

To use basic auth as a preliminary authentication method to your web property you will need to set the firewall in firewall.dat to something like:

>     AUTHENTICATORPORTS:cloudflare|ipv4|cloudflare  
>     REVERSEPROXYPORTS:443|ipv4|0.0.0.0/0
>     AUTOSCALERPORTS:
>     WEBSERVERPORTS:
>     DATABASEPORTS:

As far as the user is concerned its a pretty much standard process for a basic auth authentication requirement. They still have to be savvy enough to know that if they see a basic auth dialog that they need to authenticate by way of their email address. 


In your template to set up an authentication server if your webproperty is something like nuocial.uk and you are putting cloudflare in front of your authentication server you will need settings for basic auth something similar to:

>     ######Authentication Server#####
>     export NO_AUTHENTICATORS="1"
>     export AUTHENTICATOR_TYPE="basic-auth"
>     export AUTH_SERVER_URL="auth.nuocialsecurity.uk"
>     export AUTH_DNS_USERNAME="webmaster@nuocial.uk" (or whatever the email address for your cloudflare account is)  
>     export AUTH_DNS_SECURITY_KEY="X1234X"   (your cloudflare API key)
>     export AUTH_DNS_CHOICE="cloudflare"
>     export USER_EMAIL_DOMAIN="nuocial.uk" (the custom domain that you have issued email addresses for, for example, user1@nuocial.uk)

------------------------------

------------------------------

##### 3. Whitelist based authentication.

To use whitelist based authentication you will need to be using reverse proxy(ies) in front of your webserver(s) and you will need to allow access by default to your reverse proxies by setting an appropriate value in firewall.dat, for example:


>     AUTHENTICATORPORTS:cloudflare|ipv4|cloudflare  
>     REVERSEPROXYPORTS:443|ipv4|0.0.0.0/0
>     AUTOSCALERPORTS:
>     WEBSERVERPORTS:
>     DATABASEPORTS:

To use this method your users will have to have a certain level of sophistication but the forbidden page that is present in response to a failed login attempt will signpost the to the authentication server for them to enter their domain specific email address that you have issued them with and gain access.

In your template to set up an authentication server if your webproperty is something like nuocial.uk and you are putting cloudflare in front of your authentication server you will need settings for whitelist based authentication as something similar to:

>     ######Authentication Server#####
>     export NO_AUTHENTICATORS="1"
>     export AUTHENTICATOR_TYPE="whitelist"
>     export AUTH_SERVER_URL="auth.nuocialsecurity.uk"
>     export AUTH_DNS_USERNAME="webmaster@nuocial.uk" (or whatever the email address for your cloudflare account is)  
>     export AUTH_DNS_SECURITY_KEY="X1234X"   (your cloudflare API key)
>     export AUTH_DNS_CHOICE="cloudflare"
>     export USER_EMAIL_DOMAIN="nuocial.uk" (the custom domain that you have issued email addresses for, for example, user1@nuocial.uk)

-----------------------------------

-----------------------------------

##### 4. Wireguard based authentication servers

To use wireguard based authentication servers your users need to know that they have to have a wireguard client installed on every device that they want to access your servers through. If they have the wireguard app (for example) installed on their device then they can connect to your servers by going to the authentication servers that you have set up and following the authentication process. 

The way I have setup the wireguard system is that whatever your SSH PORT is set to your wireguard port is SSH_PORT + 1. In other words, I set my SSH_PORT to 1035 and therefore my wireguard port is 1036. This needs to be reflected in my firewall (in other words, I have to open port 1036 in my firewall). To do that I need to set the following in firewall.dat

>     AUTHENTICATORPORTS:443|ipv4|cloudflare  
>     REVERSEPROXYPORTS:1036|ipv4|0.0.0.0/0
>     AUTOSCALERPORTS:
>     WEBSERVERPORTS:
>     DATABASEPORTS:

**ESSENTIAL/MANDATORY**

This is good enough for the OS firewall but the cloud native firewall (in other words, the firewall provided by your VPS provider) if it is active, will need to explicitly open port 1036 to all UDP traffic. This is because wireguard needs to be accessible via the UDP protocol as well as the TCP protocol. I could automate this in a script but actually I thought why not leave it as a manual step that could be seen as an extra security measure in other words, you have to explicitly activate wireguard once your servers are deployed by allowing UDP through your firewall. So go ahead and allow UDP to access port 1036 (or whatever your wireguard port is) on your reverse proxy firewall. YOU USERS WON'T BE ABLE TO CONNECT TO YOUR SERVERS UNTIL YOU ALLOW UDP PACKETS THROUGH YOUR FIREWALL. Its also a useful kill switch meaning you can take your entire infrastructure offline just by blocking the UDP packets in your cloud native firewall. 

In order to setup your template so that your deployment uses wireguard you will need to configure your template similar to the following:

In your template to set up an authentication server if your webproperty is something like nuocial.uk and you are putting cloudflare in front of your authentication server you will need settings for whitelist based authentication as something similar to:

>     ######Authentication Server#####
>     export NO_AUTHENTICATORS="1"
>     export AUTHENTICATOR_TYPE="wire-guard"
>     export AUTH_SERVER_URL="auth.nuocialsecurity.uk"
>     export AUTH_DNS_USERNAME="webmaster@nuocial.uk" (or whatever the email address for your cloudflare account is)  
>     export AUTH_DNS_SECURITY_KEY="X1234X"   (your cloudflare API key)
>     export AUTH_DNS_CHOICE="cloudflare"
>     export USER_EMAIL_DOMAIN="nuocial.uk" (the custom domain that you have issued email addresses for, for example, user1@nuocial.uk)
