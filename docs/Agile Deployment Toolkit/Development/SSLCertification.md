You are welcome to alter the configuration of the webserver that you deploy. You will find the files to do that under

>     ${HOME}/services/webserver  

and you can just change the configuration files for your webserver of choice to have it deploy according to your needs.   
You can also have separate configuration sets if you want to deploy different solutions for different operational needs.   
Anyway, I developed this toolkit with a default set of configurations which may or may not fit with what you want.  
I submitted my webserver configurations for security validation and these are the results that I got.  

APACHE:  

![APACHE](./images/ssl2.png)

NGINX:  

![NGINX](./images/ssl1.png)

LIGHTTPD:  

![LIGHTTPD](./images/ssl3.png)

In other words, they look pretty much equivalent. There might be some additional steps that could bump up the minor blemishs on "key exchange" and "cipher strength" but I didn't go that far.   
