- To shutdown your infrastructure it is important not to simply shutdown the machines using a provider's GUI system or the cli tools. There's a script in the **helpers** directory called **ShutdownInfrastructure.sh** which you must run each time you want to shut your system down. This gives the machines a chance to clean up, make backups and so on which means that your data will be consistent. If your application is online and you need to shutdown your servers for some reason, then, if your most recent backup was 1 hour ago then if you don't make a "shutdown" backup then there's the potential for an hour's worth of data to be lost, worse still if you only make daily backups. This is why it is ESSENTIAL that you shutdown using the helpers. 

- If you are using Cloudflare as your DNS Service provider, you need to, at a minimum, switch on 'Full SSL' and also, you need to create a page rule which directs all calls to http://www.website.com to https://www.website.com. This way, you can be sure that all your requests are being issued securly. Also, when you are in development mode, letsencrypt will issue a "testing" certificate which Cloudflare will not accept in "strict mode". If you drop it back down to "full" during development and using staging SSL certificates and up to "strict" once you are ready for production (which doesn't use the staging SSL certificates that strict mode doesn't like), this should allow you to make most efficient use of the certificate issuing process. I say this because staging certificates have no issuing limits on them where as production ones do and if you start issuing production certificates repeatedly you will rapidly start getting error messages and failed certificate issuance. 

- Remember if you change from deploying to one DNS service and choose another, you will have to change the nameservers with the service you bought your domain names from. This is called a "Nameserver update" and has a propagation delay of up to 48 hrs before your webproperty will be accessible through the new nameservers. 

- If you want to customise the configuration of your webserver, you can easily do it by altering the scripts in

>     Agile-Infrastructure-Webserver-Scripts/services/webserver/configuration/

 
- If you are using s3fs for your shared storage, then, if you delete the buckets using the Cloudhost provider's gui system (digital ocean at least has a period when a deleted bucket cannot be recreated) or the s3cmd tool there tends to be a period of time with some providers when buckets of the same name cannot be created again. If you run the scripts during this period and they require the same bucket name as a bucket that you have recently deleted using the GUI, you will get unpredictable behaviour. If you wait till the grace period expires, then, you will be able to complete the execution of the scripts successfully or, better yet, don't delete the buckets using the gui unless you are finished with them entirely.  

- If you make multiple builds and have, for example, "testbuild-1", "testbuild-2" and so on, you need to name them (\<identifier\>-BUILD_IDENTIFIER), "1-testbuild", "2-testbuild" rather than "testbuild-1" and "testbuild-2", this is because in some places the "BUILD_IDENTIFIER" might get truncated and you would lose the distinction.    

- The Agile Deployment Toolkit supports the following application database:

>     Joomla 5 using MySQL, MariaDB or Postgres is supported  
>     Wordpress using MySQL or MariaDB is supported  (no Postgres by default with Wordpress)   
>     Drupal using MySQL, MariaDB or Postgres is supported   
>     Moodle using MySQL, MariaDB or Postgres is supported  
 
- These builds depend on external services, if a service is down, github for example, the build may well not complete.

- Be aware of firewall rules limits which are different by provider. If you had a great number of servers for some providers, the number of firewall rules might be a limiter. 
 
- This toolkit is intended to by used in such a way that managed DBs are only used when making PRODUCTION deployments. When you are in development mode it is expected that you will use the database that these scripts install on your VPS servers.  
 
- Remove termination protection for a managed database on Exoscale as follows:
 
>     exo dbaas update <db-name - eg: testdb1> -z <region: eg. ch-gva-2> --termination-protection=false

- If you are planning to deploy to a DBaaS solution then you should do your development on equivalent database types. For example if your final DBaaS deployment is to a MYSQL instance then you should baseline using MySQL as your development database type, not Maria and likewise if your final DBaaS type is Maria DB, you should develop against Maria DB rarther than MySQL. You do this by setting DATABASE_INSTALLATION_TYPE in your template

- Note that when you make a baseline of an application you have developed, you will have used a URL to develop against such as "dev.testsite.uk". What this system does is when the baseline is being generated scripts replace all occurences of dev.testsite.uk with a placeholder token which isn't domain specific for both the webroot sourcecode and the database sql script. What this means is that the baseline can be deployed to any domain then because the placeholder token is replaced with the domain name of the target domain in all sourcecode. So, the same baseline can be deployed to "www.cargarage.com" or it can be deployed to "cafe.gardencentre.uk" or any other domain you choose. In this way any baselines you generate are flexible to deploy to any domain that you choose meaning that you can create a library of baselines that can be deployed any number of times to any number of domain names.

Please note, for Wordpress, I had to make use of [serfix](https://github.com/astockwell/serfix) because of the well known serialization issue with wordpress when doing this. 

- Note, with some providers, if you want to connect using the provided console to your machines as the root user you might not be able to. Depending on the condition of the machine you are trying to connect to you maybe be able to connect using the SERVER_USERNAME or if it is a recurring problem that you can reproduce by re-deploying you may need to comment out "PermitRootLogin" and allow "PasswordAuthentication" from the cloud-init script that the machine is built using such as:

>     ${BUILD_HOME}/services/server/cloud-init/vultr/webserver.yaml

- Please be aware that no machine name should be longer than 32 characters in total, for example a machine name can be ws-lhr-test-build-1-XgzR-2MMd at most. If a machine name is longer than 32 characters then in some cases things will break because this is a hard limit of the VPS provider for machine names


