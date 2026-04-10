You can deploy an authenticator machine to your current region if you want to require your users to go through initial authentication before they access your web property. Authentication can take place using the following methods. 

#### 1. Firewall based authentication.

To use firewall based authentication you will need to be using reverse proxy(ies) in front of your webserver(s) and you will need to deny access by default to your reverse proxies by setting an appropriate value in firewall.dat, for example:


>     AUTHENTICATORPORTS:cloudflare|ipv4|cloudflare  
>     REVERSEPROXYPORTS:
>     AUTOSCALERPORTS:
>     WEBSERVERPORTS:
>     DATABASEPORTS:

if it is the case that your authenticator machine is to be accessed using cloudflare (which is recommended). 
What will then happen with this method is that when a user tries to access your webproperty their request will be blocked by the firewall. They will need to be tech savy enough to know that this curt method requires them to go to the authentication machine to enter their email address. The requirement is that you issue all your users with a custom email domain which you control (you could achieve  that by deploying [stalwart](https://stalw.art/), for example, and it is assumed that anyone with one of your custom email addresses has been vetted for access by you. And so when confronted with a timeout when accessing the web property the user knows "I must have to authenticate myself" without there being any message for them to do so and so they go to the authentication server and enter their email address. An email will be then sent to them and it will contain a link for them to enter the IP address of their laptop and the system will  then allow access to their laptop IP address to the webproperty. If their IP address changes at any point they will need to re-authenticate and establish access again in the same manner for their new IP address. To use this method your users will have to have a certain level of sophistication and understanding but you could make it clear in a simple statement. 

"If you experience any timeouts please tell us your current IP address by going to the authentication server to regain access"

NOTE: the firewall is not touched on the webserver(s) by this method the firewall access is controlled through the reverse proxy machines and so you have to be using reverse proxies for this method to be possible. 

#### 2. Basic auth based authentication

To use basic auth as a preliminary authentication method to your web property you will need to set the firewall in firewall.dat to something like:

>     AUTHENTICATORPORTS:cloudflare|ipv4|cloudflare  
>     REVERSEPROXYPORTS:443|ipv4|0.0.0.0/0
>     AUTOSCALERPORTS:
>     WEBSERVERPORTS:
>     DATABASEPORTS:

In other words, your firewall needs to allow access to your reverse proxy machines by default. When configured to use "basic auth" when the user goes to your web property they will see a "basic auth" authentication modal dialog and they will need to understand that that means they need to go to the authentication server. If anyone knows how to make it explicit what action is required when a basic auth dialog pops up please let me know. Anyway, when they go to the authentication server, they will be required to enter their email address (which must be to a domain that you control and have issued to them) and an email will be sent to them which contains a password for the basic-auth dialog box. They can then enter their email address and the password they have been given into the modal dialog box for basic auth and it if matches with what is expected, they will be granted access to the web property. As far as the user is concerned its a pretty much standard process for a basic auth authentication requirement. They still have to be savvy enough to know that if they see a basic auth dialog that they need to authenticate by way of their email address. 

Neither of these methods are utterly foolproof because they both rely on the user keeping the email account you have given them secure, but, using these methods it can help you block a lot of probing and so on which might affect your web property. 

In your template to set up an authentication server if your webproperty is something like nuocial.uk and you are putting cloudflare in front of your authentication server you will need settings for basic auth something similar to:

>     ######Authentication Server#####
>     export NO_AUTHENTICATORS="1"
>     export AUTHENTICATOR_TYPE="basic-auth"
>     export AUTH_SERVER_URL="auth.nuocialsecurity.uk"
>     export AUTH_DNS_USERNAME="webmaster@nuocial.uk" (or whatever the email address for your cloudflare account is)  
>     export AUTH_DNS_SECURITY_KEY="X1234X"   (your cloudflare API key)
>     export AUTH_DNS_CHOICE="cloudflare"
>     export USER_EMAIL_DOMAIN="nuocial.uk" (the custom domain that you have issued email addresses for, for example, user1@nuocial.uk)
