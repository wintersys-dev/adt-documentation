### MULTI REGION DEPLOYMENTS

If you want to deploy using multiple regions you will need to set

>     MULTI_REGION="1"

in your template(s) for each region.

You will also need to set a primary region by having one of the regions you are deploying to set

>     PRIMARY_REGION="1"

and all the additional regions set 


>     PRIMARY_REGION="0"


When making a multi region deployment you will need to provide the public DBaaS endpoint to all regions which are not the primary region. The primary region should be set such that the region for your webservers of your primary region is the same region/VPC/provider as the DBaaS database you are deploying.

Every region will need to provision its own reverse proxy machines because multi-region deployments are only possible when each individual region uses reverse proxies to forward traffic to the fleet of webservers for that region. 

Here I have used one build machine running  in the gb-lon to perform the builds for all regions and providers of the multi region deployment but you could have a different build machine in/for each region and that would encapsulate each region nicely and make it simpler to have different teams responsible for primary, and secondary and even tertiary regions. 

---------------------

#### EXAMPLE 1 (PRIMARY REGION Linode running gb-lon)

Shown below are my template configurations if I want to deploy a primary region of gb-lon and a secondary region of nl-ams for my application on the Linode platform.You need to take a similar approach with other providers if you want to deploy to multiple regions within the same provider.

For this example, you first need to deploy a build machine in the gb-lon region which you can do by following [this](https://www.wintersys-dev.uk/Agile%20Deployment%20Toolkit/Tutorials/linode/build-machine/)

##### TEMPLATE SETTINGS FOR THE PRIMARY REGION (gb-lon)

When you make a multi region deployment within one provider, the advice is that you should set the BUILD_IDENTIFER to include the region that is being deployed to, for example, testdeploy-gb-lon for the gb-lon domain and testdeploy-nl-ams for the nl-ams domain under linode. 

Highlighted in red are the settings in the templates that you need to take particular care with when making a multi-region deployment

##### THE TEMPLATE THAT WILL PROVISION THE MACHINES OF THE PRIMARY REGION

\###############################################################################################  
\# Refer to: ${BUILD_HOME}/specifications/specification.md
\###############################################################################################  
\#This template is configured for temporal style builds

\#####MANDATORY - the bare minimum set of values that you need to provide to have any chance of a successful build  
\#####NOT REQUIRED - isn't used by the Linode system  
     
\#####Application Settings#########  
export APPLICATION="joomla"  #MANDATORY     
export APPLICATION_BASELINE_SOURCECODE_REPOSITORY=""  
export BASELINE_DB_REPOSITORY=""  
export APPLICATION_LANGUAGE="PHP"   
export PHP_VERSION="8.4"   
export BUILD_ARCHIVE_CHOICE="hourly"  
export APPLICATION_NAME="Demo Application"  

\#####S3 Datastore Settings#######  
export S3_ACCESS_KEY="JH56HJ78WE4T6U8OO90"  
export S3_SECRET_KEY="Hgdj89K2w3eyrb1289sfjDewjk"  
export S3_HOST_BASE="nl-ams-1.linodeobjects.com"   
export S3_LOCATION="US" #For linode, this always needs to be set to "US"  
export DIRECTORIES_TO_MOUNT="images" #This should always be unset for a virgin and baseline deployments  
export PERSIST_ASSETS_TO_DATASTORE="1" #This should always be set to 0 for a virgin and baseline deployment  
     
\#####OS Settings#########  
export BUILDOS="debian" # One of ubuntu|debian  
export BUILDOS_VERSION="12" #  24.04 (or later for BUILDOS="ubuntu") | 12 (or later for BUILDOS="debian")
     
\######Cloudhost Provider Settings#######  
export TOKEN="hjdd738jdaq7fhwk2bdif8rhdnqi238fks92jdkfojshf93jsfhndjk67" #MANDATORY  
export ACCESS_KEY=""   #NOT REQUIRED  
export SECRET_KEY=""   #NOT REQUIRED  
export CLOUDHOST_ACCOUNT_ID="demoaccount"  #MANDATORY for Linode - this should be the account username that you login with  
     
\######DNS Settings##########  
export DNS_USERNAME="dnsusername@email.com"  #MANDATORY  
export DNS_SECURITY_KEY="9aa34568735212367d137dfadf34daa26:H3aYfKLqW4gfJQP1qOPc_jkWPa-QjiodJAL0jCKM"   #MANDATORY  (note you need a API token and account ID here if you use cloudflare)  
export DNS_CHOICE="cloudflare" #you will need to set your DNS nameservers according to this choice  

\#####Webserver Settings########  
export WEBSITE_DISPLAY_NAME="Joomla Demo" #MANDATORY  
export WEBSITE_NAME="codetesters" #MANDATORY  
export WEBSITE_URL="www.codetesters.uk"  #MANDATORY  
export WEBSERVER_CHOICE="APACHE"  
export REVERSE_PROXY_WEBSERVER="APACHE"  
export NO_WEBSERVERS="1"  
export MAX_WEBSERVERS="10"  
export MOD_SECURITY="0"  
     
\#####Git settings#####  
export GIT_USER="Templated User" #MANDATORY  
export GIT_EMAIL_ADDRESS="templateduser@dummyemailZ123.com" #MANDATORY  
     
\#####Infrastructure Repository Settings#######  
export INFRASTRUCTURE_REPOSITORY_PROVIDER="github"  
export INFRASTRUCTURE_REPOSITORY_OWNER="wintersys-dev"  
export INFRASTRUCTURE_REPOSITORY_USERNAME="wintersys-dev"  
     
\###### Application Repository Settings########  
export APPLICATION_REPOSITORY_PROVIDER="github"   
export APPLICATION_REPOSITORY_OWNER="adt-apps" #MANDATORY  
export APPLICATION_REPOSITORY_USERNAME="adt-apps" #MANDATORY  
export APPLICATION_REPOSITORY_TOKEN="github_pat_11BELT3NQ03fCHpjdjn7y3hjdhkf37DHHS8jfh38fjfy3o9qoskfogjJHHJDJkfhskfu3osdjf"  
     
\##### System Email Settings#########  
export SYSTEM_EMAIL_PROVIDER=""   
export SYSTEM_EMAIL_PROVIDER="2"   
export SYSTEM_TOEMAIL_ADDRESS="webmaster@codetesters.uk"   
export SYSTEM_FROMEMAIL_ADDRESS="webmaster@codetesters.uk"   
export SYSTEM_EMAIL_USERNAME="gf72fdhkocnv28de7e8ifjjw8f2"  
export SYSTEM_EMAIL_PASSWORD="hfjh47fi328rjfh28folmajfigj"  
export EMAIL_NOTIFICATION_LEVEL="ERROR"  
     
\##### Database Settings######  
export DB_PORT="2035"  
<span style="color:red">export DATABASE_INSTALLATION_TYPE="DBaaS"</span>    
<span style="color:red">export DATABASE_DBaaS_INSTALLATION_TYPE="MySQL:DBAAS:mysql/8:gb-lon:g6-nanode-1:1:test-cluster:testdb:testdbuser:hfhuf83jfhfu73jd:436653:234531" </span>     
<span style="color:red">export DB_INSTALL_MODE="1"</span>       
    
\#####Server Settings #######  
<span style="color:red"> export REGION="gb-lon"</span>   
export DB_SERVER_TYPE="g6-nanode-1"  
export WS_SERVER_TYPE="g6-nanode-1"   
export AS_SERVER_TYPE="g6-nanode-1"  
export AUTH_SERVER_TYPE="g6-nanode-1"  
export RP_SERVER_TYPE="g6-nanode-1"   
export MACHINE_TYPE="LINODE"  
export SSH_PORT="1035"  
export SERVER_TIMEZONE_CONTINENT="Europe"  
export SERVER_TIMEZONE_CITY="London"  
export USER="root"  
export SYNC_WEBROOTS="0"  
export USER_EMAIL_DOMAIN=""  
     
\#####Build Settings######   
<span style="color:red">export DEPLOYMENT_MODE="PRODUCTION" </span>   
<span style="color:red">export NO_AUTOSCALERS="1"  </span>   
<span style="color:red">export NO_REVERSE_PROXIES="1"</span>   
export AUTHENTICATION_SERVER="0"  
export BUILD_FROM_SNAPSHOT="0"  
     
\#####Security Settings#####  
export ACTIVE_FIREWALLS="3"  
export PUBLIC_KEY_NAME="AGILE_TOOLKIT_PUBLIC_KEY"  
export SSL_GENERATION_METHOD="AUTOMATIC"  
export SSL_GENERATION_SERVICE="LETSENCRYPT"  
export SSL_LIVE_CERT="1"  
export ALGORITHM="ed25519"  
<span style="color:red">export BUILD_MACHINE_VPC="1"</span>  
export VPC_IP_RANGE="10.0.1.0/24"  
<span style="color:red">export VPC_NAME="adt-vpc-gb-lon" </span>  
     
\#####Multi Region Deployments#####  
<span style="color:red">export MULTI_REGION="1"</span>   
<span style="color:red">export PRIMARY_REGION="1"</span>    
export DBaaS_PUBLIC_ENDPOINT=""  
     
\#####Build Style#######  
export IN_PARALLEL="1"  

<span style="color:red">export BUILD_IDENTIFIER="test-gb-lon"  </span>  
export CLOUDHOST="linode"  


---------------------------

Now we need a template for the nl-ams region when I have  deployed the machines I need to a primary region of gb-lon using the template above.

##### THE TEMPLATE THAT WILL PROVISION THE MACHINES OF THE SECONDARY REGION

\###############################################################################################  
\# Refer to: ${BUILD_HOME}/specifications/specification.md  
\###############################################################################################  
\#This template is configured for temporal style builds  
     
\#####MANDATORY - the bare minimum set of values that you need to provide to have any chance of a successful build  
\#####NOT REQUIRED - isn't used by the Linode system  
    
\#####Application Settings#########  
export APPLICATION="joomla"  #MANDATORY    
export APPLICATION_BASELINE_SOURCECODE_REPOSITORY=""  
export BASELINE_DB_REPOSITORY=""   
export APPLICATION_LANGUAGE="PHP"   
export PHP_VERSION="8.4"   
export BUILD_ARCHIVE_CHOICE="hourly"  
export APPLICATION_NAME="Demo Application"  
     
\#####S3 Datastore Settings#######  
export S3_ACCESS_KEY="JH56HJ78WE4T6U8OO90"  
export S3_SECRET_KEY="Hgdj89K2w3eyrb1289sfjDewjk"  
export S3_HOST_BASE="nl-ams-1.linodeobjects.com"   
export S3_LOCATION="US" #For linode, this always needs to be set to "US"  
export DIRECTORIES_TO_MOUNT="images" #This should always be unset for a virgin and baseline deployments  
export PERSIST_ASSETS_TO_DATASTORE="1" #This should always be set to 0 for a virgin and baseline deployment  
     
\#####OS Settings#########  
export BUILDOS="debian" # One of ubuntu|debian  
export BUILDOS_VERSION="12" #  24.04 (or later for BUILDOS="ubuntu") | 12 (or later for BUILDOS="debian")  
     
\######Cloudhost Provider Settings#######  
export TOKEN="hjdd738jdaq7fhwk2bdif8rhdnqi238fks92jdkfojshf93jsfhndjk67" #MANDATORY  
export ACCESS_KEY=""   #NOT REQUIRED  
export SECRET_KEY=""   #NOT REQUIRED  
export CLOUDHOST_ACCOUNT_ID="demoaccount"  #MANDATORY for Linode - this should be the account username that you login with  
     
\######DNS Settings##########  
export DNS_USERNAME="dnsusername@email.com"  #MANDATORY  
export DNS_SECURITY_KEY="9aa34568735212367d137dfadf34daa26:H3aYfKLqW4gfJQP1qOPc_jkWPa-QjiodJAL0jCKM"   #MANDATORY  (note you need a API token and account ID here if you use cloudflare)  
export DNS_CHOICE="cloudflare" #you will need to set your DNS nameservers according to this choice  
     
\#####Webserver Settings########  
export WEBSITE_DISPLAY_NAME="Joomla Demo" #MANDATORY  
export WEBSITE_NAME="codetesters" #MANDATORY  
export WEBSITE_URL="www.codetesters.uk"  #MANDATORY  
export WEBSERVER_CHOICE="APACHE"  
export REVERSE_PROXY_WEBSERVER="APACHE"  
export NO_WEBSERVERS="1"  
export MAX_WEBSERVERS="10"  
export MOD_SECURITY="0"  
     
\#####Git settings#####  
export GIT_USER="Templated User" #MANDATORY  
export GIT_EMAIL_ADDRESS="templateduser@dummyemailZ123.com" #MANDATORY  
     
\#####Infrastructure Repository Settings#######  
export INFRASTRUCTURE_REPOSITORY_PROVIDER="github"  
export INFRASTRUCTURE_REPOSITORY_OWNER="wintersys-dev"  
export INFRASTRUCTURE_REPOSITORY_USERNAME="wintersys-dev"  
     
\###### Application Repository Settings########  
export APPLICATION_REPOSITORY_PROVIDER="github"  
export APPLICATION_REPOSITORY_OWNER="adt-apps" #MANDATORY  
export APPLICATION_REPOSITORY_USERNAME="adt-apps" #MANDATORY  
export APPLICATION_REPOSITORY_TOKEN="github_pat_11BELT3NQ03fCHpjdjn7y3hjdhkf37DHHS8jfh38fjfy3o9qoskfogjJHHJDJkfhskfu3osdjf"  
     
\##### System Email Settings#########  
export SYSTEM_EMAIL_PROVIDER=""  
export SYSTEM_EMAIL_PROVIDER="2"   
export SYSTEM_TOEMAIL_ADDRESS="webmaster@codetesters.uk"  
export SYSTEM_FROMEMAIL_ADDRESS="webmaster@codetesters.uk"   
export SYSTEM_EMAIL_USERNAME="gf72fdhkocnv28de7e8ifjjw8f2"  
export SYSTEM_EMAIL_PASSWORD="hfjh47fi328rjfh28folmajfigj"  
export EMAIL_NOTIFICATION_LEVEL="ERROR"  
     
\##### Database Settings######  
export DB_PORT="2035"  
<span style="color:red">export DATABASE_INSTALLATION_TYPE="DBaaS"</span> 
<span style="color:red">export DATABASE_DBaaS_INSTALLATION_TYPE="MySQL:DBAAS:mysql/8:gb-lon:g6-nanode-1:1:test-cluster:testdb:testdbuser:hfhuf83jfhfu73jd:436653:234531" </span>  
<span style="color:red">export DB_INSTALL_MODE="2"</span>       

\#####Server Settings #######  
<span style="color:red">export REGION="nl-ams" </span>  
export DB_SERVER_TYPE="g6-nanode-1"  
export WS_SERVER_TYPE="g6-nanode-1"  
export AS_SERVER_TYPE="g6-nanode-1"  
export AUTH_SERVER_TYPE="g6-nanode-1"  
export RP_SERVER_TYPE="g6-nanode-1"  
export MACHINE_TYPE="LINODE"  
export SSH_PORT="1035"  
export SERVER_TIMEZONE_CONTINENT="Europe"  
export SERVER_TIMEZONE_CITY="London"  
export USER="root"  
export SYNC_WEBROOTS="0"  
export USER_EMAIL_DOMAIN=""  
     
\#####Build Settings######  
<span style="color:red">export DEPLOYMENT_MODE="PRODUCTION" </span>   
<span style="color:red">export NO_AUTOSCALERS="1"  </span>   
<span style="color:red">export NO_REVERSE_PROXIES="1"</span>    
export AUTHENTICATION_SERVER="0"  
export BUILD_FROM_SNAPSHOT="0"  
     
\#####Security Settings#####  
export ACTIVE_FIREWALLS="3"  
export PUBLIC_KEY_NAME="AGILE_TOOLKIT_PUBLIC_KEY"  
export SSL_GENERATION_METHOD="AUTOMATIC"  
export SSL_GENERATION_SERVICE="LETSENCRYPT"  
export SSL_LIVE_CERT="1"  
export ALGORITHM="ed25519"  
<span style="color:red">export BUILD_MACHINE_VPC="0" </span>    
export VPC_IP_RANGE="10.0.1.0/24"  
<span style="color:red">export VPC_NAME="adt-vpc-nl-ams" </span>    
     
\#####Multi Region Deployments#####  
<span style="color:red">export MULTI_REGION="1"  </span>  
<span style="color:red">export PRIMARY_REGION="0" </span>  
<span style="color:red">export DBaaS_PUBLIC_ENDPOINT="public-a47568393-akamai-prod-6748387-default.g2a.akamaidb.net" </span>  
     
\#####Build Style#######  
export IN_PARALLEL="1"  
          
<span style="color:red">export BUILD_IDENTIFIER="test-nl-ams"</span>    
export CLOUDHOST="linode"  

You will need to first deploy the primary region infrastrucuture (gb-lon) and once that is complete and online, deploy the secondary region and any further regions that you want to deploy for. The DBaaS instance is in the gb-lon region and the machines in nl-ams will connect to the database instance in the gb-lon domain across the Internet. As I understand it the traffic is encrypted by default but to be sure don't take my word for it check it out with Linode because it might be necessary to provide SSL certs when connecting to the DBaaS system across the Internet, I am no expert. 

Once, I had my machines running in a primary and secondary region within the Linode provider I then made a deployment to a third region (with a different vendor, Exoscale) and below is what my template looked like when I made a deployment to a different vendor and had all the different regions tied together into a unified interface through the Cloudflare DNS system and all having them access the shared Linode DBaaS system. Remember I chose to intitiate all these builds (including the Exoscale machines) from the same build machine running on Linode but with the different template configurations that I have shown here. 

\###############################################################################################  
\# Refer to: ${BUILD_HOME}/specifications/specification.md  
\###############################################################################################  
\#This template is configured for temporal style builds  

\#####MANDATORY - The bare minimum set of values that you need to provide to have any chance of a successful build  
\#####NOT REQUIRED - isn't used by the Exoscale  

\#####Application Settings#########  
export APPLICATION="joomla" #MANDATORY    
export APPLICATION_BASELINE_SOURCECODE_REPOSITORY=""  
export BASELINE_DB_REPOSITORY=""  
export APPLICATION_LANGUAGE="PHP"   
export PHP_VERSION="8.4"    
export BUILD_ARCHIVE_CHOICE="hourly"  
export APPLICATION_NAME="Demo Application"  

\#####S3 Datastore Settings#######  
export S3_ACCESS_KEY="GPIJ6HS1MY6LU7QE243V"  
export S3_SECRET_KEY="Ru9aX4oBxK3W1Ga1eTNj4c96SV8AQzsk4p6KXdvT"  
export S3_HOST_BASE="nl-ams-1.linodeobjects.com"   
export S3_LOCATION="US" #For linode, this always needs to be set to "US"  
export DIRECTORIES_TO_MOUNT="images" #This should always be unset for a virgin and baseline deployments  
export PERSIST_ASSETS_TO_DATASTORE="1" #This should always be set to 0 for a virgin and baseline deployment  

\#####OS Settings#########  
export BUILDOS="debian" # One of ubuntu|debian  
export BUILDOS_VERSION="12" #  24.04 (or later for BUILDOS="ubuntu") | 12 (or later for BUILDOS="debian")

\######Cloudhost Provider Settings#######  
export TOKEN="" #NOT REQUIRED  
export ACCESS_KEY="EXO7hfgu476gfvfnsu4nfbgy223dhf"   #MANDATORY  
export SECRET_KEY="jcbh846tfg2vdugihdhevuft2ojknbub374fbjbsksbd"   #MANDATORY  
export CLOUDHOST_ACCOUNT_ID="testcoders@yahoo.com"  #MANDATORY for Exoscale - this should be the account email address that you login to the portal with  

\######DNS Settings##########  
export DNS_USERNAME="dnsusername@email.com" #MANDATORY  
export DNS_SECURITY_KEY="9aa34568735212367d137dfadf34daa26:H3aYfKLqW4gfJQP1qOPc_jkWPa-QjiodJAL0jCKM"   #MANDATORY  (note you need a API token and account ID here if you use cloudflare)   
export DNS_CHOICE="cloudflare" #you will need to set your DNS nameservers according to this choice  

\#####Webserver Settings########  
export WEBSITE_DISPLAY_NAME="Joomla Demo" #MANDATORY  
export WEBSITE_NAME="codetesters" #MANDATORY  
export WEBSITE_URL="www.codetesters.uk"  #MANDATORY  
export WEBSERVER_CHOICE="APACHE"  
export REVERSE_PROXY_WEBSERVER="APACHE"  
export NO_WEBSERVERS="1"  
export MAX_WEBSERVERS="10"  
export MOD_SECURITY="0"  

\#####Git settings#####  
export GIT_USER="Templated User"  
export GIT_EMAIL_ADDRESS="templateduser@dummyemailZ123.com"  

\#####Infrastructure Repository Settings#######  
export INFRASTRUCTURE_REPOSITORY_PROVIDER="github"  
export INFRASTRUCTURE_REPOSITORY_OWNER="wintersys-dev"  
export INFRASTRUCTURE_REPOSITORY_USERNAME="wintersys-dev"  

\###### Application Repository Settings########  
export APPLICATION_REPOSITORY_PROVIDER="github"  
export APPLICATION_REPOSITORY_OWNER="adt-apps" #MANDATORY  
export APPLICATION_REPOSITORY_USERNAME="adt-apps" #MANDATORY  
export APPLICATION_REPOSITORY_TOKEN="github_pat_11BELT3NQ03fCHpjdjn7y3hjdhkf37DHHS8jfh38fjfy3o9qoskfogjJHHJDJkfhskfu3osdjf"  

\##### System Email Settings#########  
export SYSTEM_EMAIL_PROVIDER=""  
export SYSTEM_EMAIL_PROVIDER="2"  
export SYSTEM_TOEMAIL_ADDRESS="webmaster@codetesters.uk"  
export SYSTEM_FROMEMAIL_ADDRESS="webmaster@codetesters.uk"  
export SYSTEM_EMAIL_USERNAME="gf72fdhkocnv28de7e8ifjjw8f2"  
export SYSTEM_EMAIL_PASSWORD="hfjh47fi328rjfh28folmajfigj"  
export EMAIL_NOTIFICATION_LEVEL="ERROR"  

\##### Database Settings######  
export DB_PORT="2035"  
export DATABASE_INSTALLATION_TYPE="DBaaS"  
export DATABASE_DBaaS_INSTALLATION_TYPE="MySQL:DBAAS:mysql/8:gb-lon:g6-nanode-1:1:test-cluster:testdb:testdbuser:hfhuf83jfhfu73jd"  
<span style="color:red">export DB_INSTALL_MODE="2"</span>       

\#####Server Settings #######  
<span style="color:red">export REGION="ch-gva-2" </span>  
export DB_SERVER_TYPE="tiny"  
export WS_SERVER_TYPE="tiny"  
export AS_SERVER_TYPE="tiny"  
export AUTH_SERVER_TYPE="tiny"  
export RP_SERVER_TYPE="tiny"  
export MACHINE_TYPE="EXOSCALE"  
export SSH_PORT="1035"  
export SERVER_TIMEZONE_CONTINENT="Europe"  
export SERVER_TIMEZONE_CITY="London"  
export USER="root"  
export SYNC_WEBROOTS="1"  
export USER_EMAIL_DOMAIN=""  

\#####Build Settings######  
export DEPLOYMENT_MODE="PRODUCTION"  
export NO_AUTOSCALERS="1"  
<span style="color:red">export NO_REVERSE_PROXIES="1" </span>   
export AUTHENTICATION_SERVER="0"  
export BUILD_FROM_BACKUP="0"  
 
\#####Security Settings#####  
export ACTIVE_FIREWALLS="3"  
export PUBLIC_KEY_NAME="AGILE_TOOLKIT_PUBLIC_KEY"  
export SSL_GENERATION_METHOD="AUTOMATIC"  
export SSL_GENERATION_SERVICE="LETSENCRYPT"  
export SSL_LIVE_CERT="1"  
export ALGORITHM="rsa"  
<span style="color:red">export BUILD_MACHINE_VPC="0" </span>  
export VPC_IP_RANGE="10.0.0.0/24"  #MANDATORY   
export VPC_NAME="adt-vpc"  

\#####Multi Region Deployments#####  
<span style="color:red">export MULTI_REGION="1" </span>  
<span style="color:red">export PRIMARY_REGION="0" </span>  
<span style="color:red">export DBaaS_PUBLIC_ENDPOINT="a327564-akamai-prod-370219-default.g2a.akamaidb.net" </span>  


\#####Build Style#######  
export IN_PARALLEL="1"  

export BUILD_IDENTIFIER="test-ch-gva"  
export CLOUDHOST="exoscale"  

---------------------------------

#### EXAMPLE 2 (PRIMARY REGION Exoscale running ch-gva)

In this example, (without showing all the detailed templates which I showed above) I made a deployment across 4 diffferent vendors with the Exoscale vendor as the PRIMARY_REGION with the machine marked "VM....." as the build machine from which the servers of all vendors are built and deployed from. In other words, its the machine on which the templates are configured.  The DBaaS service used in this example is the DBaaS service offering from Exoscale and all the other vendors connect in to the DBaaS deployed through the primary domain mechanism across the Internet (performance seems to be OK as far as it goes).

![](images/exoscale-servers.png "Exoscale Servers Image")

The Digital Ocean servers are running as a secondary region. 
![](images/digitalocean-servers.png "Digital Ocean Servers Image")

The Linode servers are running as a 3rd region. 

![](images/linode-servers.png "Linode Servers Image")

The Vultr servers are running as a 4th region

![](images/vultr-servers.png "Vultr Servers Image")

If you notice that there's a reverse proxy machine on for each vendor and as there are 4 vendors with one reverse proxy each we would expect there to be four ip addresses added to the DNS system and there are. If you look closely you will see that the Cloudflare DNS IP addresses are the IP addresses of the reverse proxy machines for each region. 

![](images/cloudflare-dns.png "Cloudflare DNS Image")

In a live deployment you would most likely want at least 2 reverse proxies per region for resilience. I might as well note here as well that you will see that only Exoscale has a bespoke DB layer deployed which is a single point of failure (in terms of backups and so on) so in a live system you will most likely want a DB layer on a second region as well, which you can do by setting DB_INSTALL_MODE to 2 ("2" because the primary domain is active already) although you could also set it to zero but this might leave a single point of failure (in terms of application backups but not in terms of application function)  because you only have the bespoke database VPS running in the primary region and not in any of the additional regions.

You can see the DBaaS system on Exoscale has had its ip-filter set to allow the database (VPS) machine runnning on the Exoscale server fleet (ip address: 185.19.28.170) to have access to the DBaaS system along with all the webserver ip addresses acrosss all the vendors. This list of IP addresses that have been granted access to our DBaaS system (the rest of the world is firewalled off) look as follows showing our 6 ip addresses that have been granted access to the DBaaS system:

![](images/dbaas-ipfilter.png "Exoscale DBaaS ip-filter Image")

So, for fun I scaled up the server fleet running on the Vultr vendor to 5 webservers which you can see here

![](images/vultr-servers-scaledup.png "Vultr Servers Scaledup")

And then you can see that the scaling up process opens up the ip-filter of the DBaaS system to allow the ip addresses of our new webservers as you can see here:

![](images/dbaas-ipfilter-scaleup.png "Exoscale DBaaS ip-filter scale up Image")

Here is shown 10 ip addresses that are allowed to access the database. 

I then ran a one off apache bench test and here are the results:

![](images/apache-bench-profiling.png "Apache Bench Image")

------------------------------

### RESILIENCE IN MULTI REGION DEPLOYMENTS

In the examples I have given, there is one build machine in one region and all the other regions on different vendors are built from that one build machine. If you want maximum resilience, then, what you need to do is have a build machine for each region that you are deploying to. That way a whole region can be taken offline (whilst other regions are kept online) with no difference in result to the end user. If a region has a problem then it can be swapped out and rebuilt or deployed to a different region if there is a regional issue and so on. This makes the way you are approaching your deployment resilient to regional outages because if you only have one build machine like I have done with these examples and if you are deploying to different vendors, an outage at a particular provider might snooker your ability to build or scale in all your regions. 

-------------------------

### MULTI REGION DBaaS DEPLOYMENTS 

(Not currently supported but could be with some work (and money)- I don't have the money to deploy expensive cross regional databases for testing purposes 30 quid a month is about my budget for this project during its development.

If you really wanted to go to town it might be possible to use "multi region DBaaS deployments" by integrating AWS Aurora making your application accessible with low latency globally. The solution I have provided is only really suitable for specific geographical regions as it stands because the "database" is only located in a particular geographic location or region which means that if you go on holiday to Australia and you want to access an application local to the UK (in other words, a DBaaS solution local to the UK, for example). Without multi-region databases you will have high latency problems. 

[Amazon Aurora Global Database](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html)  
[Amazon Aurora Write Forwarding](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database-write-forwarding.html)  

