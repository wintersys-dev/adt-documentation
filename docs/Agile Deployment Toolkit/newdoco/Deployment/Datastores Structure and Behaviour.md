#### The nomenclature of the various datastore buckets that they system uses is as follows:

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
