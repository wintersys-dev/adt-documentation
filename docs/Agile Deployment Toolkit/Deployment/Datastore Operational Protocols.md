#### DATASTORE OPERATIONS

I provide tooling to facilitate the following operational interactions with the datastore system


>     ${BUILD_HOME}/services/datastore/operations/DeleteFromDatastore.sh

This will delete a specified object from the datastore and can be applied across multiple regions from one invocation as described below

>     ${BUILD_HOME}/services/datastore/operations/GetFromDatastore.sh

This will obtain a specified object and should only be applied to the local region as described below

>     ${BUILD_HOME}/services/datastore/operations/ListFromDatastore.sh

This will list a specified object and should only be applied to the local region as described below

>     ${BUILD_HOME}/services/datastore/operations/MountDatastore.sh

This will mount a datastore of a given name and can be applied across multiple regions from one invocation as described below

>     ${BUILD_HOME}/services/datastore/operations/MoveDatastore.sh

This will move an object inside a datastore and can be applied across multiple regions from one invocation as described below

>     ${BUILD_HOME}/services/datastore/operations/PutToDatastore.sh

This will put an object from the local filesystem into the datastore and can be applied across multiple regions in from one invocation as described below

>     ${BUILD_HOME}/services/datastore/operations/SyncDatastore.sh

This will sync between datastores of different names 

>     ${BUILD_HOME}/services/datastore/operations/SyncFromDatastore.sh

This will sync the given datastore to the local filesystem and should only be applied to the local region as described below

>     ${BUILD_HOME}/services/datastore/operations/SyncToDatastore.sh

This will sync from the local filesystem to a datastore and can be applied across multiple regions from one invocation as described below

--------------------

#### DATASTORE NOMENCLATURE

The nomenclature of the various datastore buckets that they system uses is as follows:

>     <website_url>-<dns-choice>-<service_token>-ssl (bucket for holding SSL certificates for a specific URL and domain service)
>     <website_url>-multi-region  (bucket for holding any inter region data, in other words if you want to pass data between regions you can persist it here)
>     <website_url>-webroot-sync-tunnel-<additional_specifier> (used for syncing the webroots of webserver machines)
>     <website_url>-config-sync-tunnel-<additional_specifier> (used for syncing the config settings between machines when in heavyweight or lightweight mode)
>     <website_url>-config-<token>                            (config bucket where the configuration settings for the current machine are kept)
>     <website_url>-assets-<additional_specifier>             (webserver assets can be kept here and made available for access using various toolings)
>     authip-adt-allowed-<additional_specifier>               (this is where the Laptop IPs that have access to the build machine are stored, add an IP address here to grant that machine access)
>     <website-url>-<dns-choice>-dbaas                        (data related to the managed database system is stored here)
>     <website-url>-<dns-choice>-snap                         (this will retain data related to snapshots)
>     <website_url>-firewall-auth-laptop-ips                  (this defines which laptop ip addresses are allowed to access the reverse proxy machines when firewall based authentication servers are active)
>     <website_url>-basic-auth-credentials                    (this defines the basic auth credentials for access to the reverse proxy servers when basic auth based authentication servers are active)
>     <website_url>-wireguard-config                          (this defines the wireguard config files for access to the reverse proxy servers when wiregard based authentication servers are active)

--------------------------------

#### DATASTORE CONFIGURATIONS

This toolkit allows for cross region sychronisation of various datastore buckets. The way it does this is by simply iterating across a set of configured regions for the current class of datastore bucket and applying the currently request action across each of the regions in turn. Its not difficult to set the system up to be multi-region aware.

##### Single region datastores

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


##### Multiple Region datastores 

(this means that by chaining regions in this way every datastore interaction will operate across each region in turn)

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

You can chain regions to any level you could have a chain of six or more regions if you wanted to but remmeber that each interaction with the datastore will then require six datastore accesses across all six regions which will by definition will always be remote to your current region

Multi Region and Multi Provider (this means that datastores will be replicated across regions and across providers)

Lets use Digital Ocean AMS as our default region and GVA Exoscale, LON Linode and SGP Vultr as our secondary regions/providers

>     export S3_ACCESS_KEY="<AMS access key generated using the gui>|<GVA access key generated using the gui>|<LON access key generated using the gui>|<SGP access key generated using the gui>"  
>     export S3_SECRET_KEY="<AMS secret key generated using the gui>|<GVA secret key generated using the gui>|<LON secret key generated using the gui>|<SGP secret key generated using the gui>" 
>     export S3_HOST_BASE="ams3.digitaloceanspaces.com|sos-ch-gva-2.exo.io|gb-lon-1.linodeobjects.com|sgp1.vultrobjects.com"
>     export S3_LOCATION="US|US|US|US" 

This configuration means that your datastores are replicated to Digital Ocean AMS Exoscale GVA Linode LON and Vultr SGP giving you four replications of your data across different providers. What this means is that any servers that you have got active for digital ocean will access the digital ocean region buckets, any servers that you have got active for exoscale will access the exoscale buckets and so on. The example configuration I have given is how you would configure the datastore chain for your digital ocean servers. You need to have your local datastore configured first in the list. So, the example already given is for serviers local to digital ocean amsterdam for any servers that you have got running on exoscale you would configure your datastore chain with exoscale first, in other words:

>     export S3_ACCESS_KEY="<GVA access key generated using the gui>|<AMS access key generated using the gui>|<LON access key generated using the gui>|<SGP access key generated using the gui>"  
>     export S3_SECRET_KEY="<GVA secret key generated using the gui>|<AMS secret key generated using the gui>|<GVA secret key generated using the gui>|<LON secret key generated using the gui>|<SGP secret key generated using the gui>" 
>     export S3_HOST_BASE="sos-ch-gva-2.exo.io|ams3.digitaloceanspaces.com|gb-lon-1.linodeobjects.com|sgp1.vultrobjects.com"
>     export S3_LOCATION="US|US|US|US" 

To make it clear by placing your local buckets first in the chain of datastore credentials the system considers the first bucket the reference bucket for that region which you want to be local to your servers, naturally. 

To give you a simple code example to show you how the system references the first indexed entry as its local datasource you can look at the following code

>     ${datastore_tool} --credentials-file /root/.s5cfg-1 --endpoint-url https://${host_base} ls s3://${active_bucket}/

In the:  

>     --credentials-file /root/.s5cfg-1 

The s5cfg-1 references the first entry in the chain

Another relevant piece of code which might aid with understanding is:

>     S3_ACCESS_KEY="`${HOME}/utilities/config/ExtractConfigValue.sh 'S3ACCESSKEY'`"
>     no_tokens="`/bin/echo "${S3_ACCESS_KEY}" | /usr/bin/fgrep -o '|' | /usr/bin/wc -l`"
>     no_tokens="`/usr/bin/expr ${no_tokens} + 1`"
>     
>     count="1"
>     
>     if ( [ "${mode}" = "local" ] )
>     then
>             ${HOME}/services/datastore/operations/PerformPutToDatastore.sh ${file_to_put} ${active_bucket}/${place_to_put} ${delete} ${count}
>     elif ( [ "${mode}" = "distributed" ] )
>     then
>             while ( [ "${count}" -le "${no_tokens}" ] )
>             do
>                     ${HOME}/services/datastore/operations/PerformPutToDatastore.sh ${file_to_put} ${active_bucket}/${place_to_put} "no" ${count}
>                     count="`/usr/bin/expr ${count} + 1`"
>             done
>     fi
