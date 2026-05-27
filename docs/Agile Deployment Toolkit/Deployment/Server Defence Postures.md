#### EXAMPLE SERVER DEFENCE POSTURES

Here are some example ways that you might want to configure your servers' defence postures for application usage and how these configurations behave and what is and isn't possible with each configuration.

1. An autoscaler and webservers connected to a database server (DBaaS or self managed)

In this configuration you will likely want to use Cloudflare or some alternative service (which you will have to add suport for). The reason for wanting to put cloudflare in front of your webserves is that the various techniques that are possible using this toolkit such as basic-auth, firewall limitation and wireguard protection aren't available for the webserver machines. These techniques require the use of reverse proxy machines. You will need to set the webservers to be accessible to cloudflare associatd IP addresses which you can do in the 

>     ${BUILD_HOME}/configuration/firewall.dat 

file.

2. Authentication server with a single reverse proxy, autoscaler, webservers and a database server

In this configuration you can configure your reverse proxy machines with your choice of protections. You can set your reverse proxy to be protected using basic-auth, firewall blocking or wireguard. The disadvantage is that you only have one reverse proxy which is a single point of failure but by having a reverse proxy you have our full armoury of techniques to protect your servers at your disposal. 
You will likely want cloudflare in front of your authentication server so that you have a defensive proxy in front of it. 

3. Authentication server with multiple reverse proxies, autoscalers, webservers and a database server

When you have this configuration (multiple reverse proxies) you will need to have a loadbalancer in front of your reverse proxies. This loadbalancer will have to be configured to use TCP connections and sticky sessions. Each client request is then routed by the loadbalancer to the same reverse proxy thereby preserving the session. The loadbalancer is intended to be started and configured using the GUI system of your cloudhost provider. If you have a loadbalancer that procludes the use of the firewalling authentication technique to allow individual IP addresses but you can still use the basic-auth and wireguard technique. If you want to you can use cloudflare for your DNS system and whatever your DNS provider turns out to be the only IP address you will need to add to the DNS system is the IP address of the loadbalancer. If you put cloudflare in front of your loadbalancer you can create and add a firewall for your loadbalancer and add the cloudflare IP addresses to the firewall. If you use a loadbalancer it will connect to your reverse proxy machines in response to a request through the VPC private network and so your reverse proxy machines can be configured to be completely firewalled off in 

>     ${BUILD_HOME}/configuration/firewall.dat
