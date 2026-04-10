DATASTORE TEMPLATE CONFIGURATION

Single region datastore

If you are just using a datastore for one region (which has to be the same region as your webservers) then you can configure the datastore in your template as follows


Digital Ocean

Lets use the Amsterdam region as our chosen region 

>     export S3_ACCESS_KEY="<access key generated using the gui>"  
>     export S3_SECRET_KEY="<secret key generated using the gui>"  
>     export S3_HOST_BASE="ams3.digitaloceanspaces.com"
>     export S3_LOCATION="US" 


Exoscale

Lets use the Geneva region as our chosen region

>     export S3_ACCESS_KEY="<access key generated using the gui>"  
>     export S3_SECRET_KEY="<secret key generated using the gui>"  
>     export S3_HOST_BASE="sos-ch-gva-2.exo.io" 
>     export S3_LOCATION="US" 

Linode

Lets use the London region as our chosen region

>     export S3_ACCESS_KEY="<access key generated using the gui>"  
>     export S3_SECRET_KEY="<secret key generated using the gui>"  
>     export S3_HOST_BASE="gb-lon-1.linodeobjects.com" 
>     export S3_LOCATION="US" 

Vultr

Lets use the Amsterdam region as our chosen region

>     export S3_ACCESS_KEY="<access key generated using the gui>"  
>     export S3_SECRET_KEY="<secret key generated using the gui>"  
>     export S3_HOST_BASE="ams1.vultrobjects.com"
>     export S3_LOCATION="US" 

Multiple Region datastores (this means that backups will be stored across regions for resilience and so on)

Lets use Amsterdam as our default region with Frankfurt as our secondary region

>     export S3_ACCESS_KEY="<AMS access key generated using the gui>|<FRA access key generated using the gui>"  
>     export S3_SECRET_KEY="<AMS secret key generated using the gui>|<FRA secret key generated using the gui>""  
>     export S3_HOST_BASE="ams3.digitaloceanspaces.com|fra1.digitaloceanspaces.com"
>     export S3_LOCATION="US|US" 

Exoscale

Lets use the Geneva region as our default region and Munich as our secondary region

>     export S3_ACCESS_KEY="<GVA access key generated using the gui>|<MUC access key generated using the gui>"  
>     export S3_SECRET_KEY="<GVA secret key generated using the gui>|<MUC secret key generated using the gui>"  
>     export S3_HOST_BASE="sos-ch-gva-2.exo.io|sos-de-muc-1.exo.io" 
>     export S3_LOCATION="US|US" 

Linode

Lets use the London region as our default region and Amsterdam as our secondary region

>     export S3_ACCESS_KEY="<LON access key generated using the gui>|<AMS access key generated using the gui>"  
>     export S3_SECRET_KEY="<LON secret key generated using the gui>|<AMS secret key generated using the gui>"  
>     export S3_HOST_BASE="gb-lon-1.linodeobjects.com|nl-ams-1.linodeobjects.com" 
>     export S3_LOCATION="US|US" 

Vultr

Lets use the Amsterdam region as our default region and Singapore as our secondary region

>     export S3_ACCESS_KEY="<AMS access key generated using the gui>|<SGP access key generated using the gui>"  
>     export S3_SECRET_KEY="<AMS secret key generated using the gui>|<SGP secret key generated using the gui>"  
>     export S3_HOST_BASE="ams1.vultrobjects.com|sgp1.vultrobjects.com"
>     export S3_LOCATION="US|US" 

You can chain regions to any level you could have a chain of six or more regions if you wanted to

Multi Region Multi Provider (this means that datastores will be replicated across regions and across providers)

Lets use Digital Ocean AMS as our default region and GVA Exoscale, LON Linode and SGP Vultr as our secondary regions/providers

>     export S3_ACCESS_KEY="<AMS access key generated using the gui>|<GVA access key generated using the gui>|<LON access key generated using the gui>|<SGP access key generated using the gui>"  
>     export S3_SECRET_KEY="<AMS secret key generated using the gui>|<GVA secret key generated using the gui>|<LON secret key generated using the gui>|<SGP secret key generated using the gui>" 
>     export S3_HOST_BASE="ams3.digitaloceanspaces.com|sos-ch-gva-2.exo.io|gb-lon-1.linodeobjects.com|sgp1.vultrobjects.com"
>     export S3_LOCATION="US|US|US|US" 

This configuration means that your datastores are replicated to Digital Ocean AMS Exoscale GVA Linode LON and Vultr SGP giving you four backups in four different regions with four different providers. 
