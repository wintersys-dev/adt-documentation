
### APPLICATION ASSETS STORAGE

#### AN APPLICATION WITH NOT MANY ASSETS 

Your application doesn't have or expect to have many assets, for example if its a personal blog you might not ever create more than a couple of GB of asset data so you can simply use the webserver's native filesystem to store your assets without any hassle at all. If you have multiple webservers then you will need to set

>     WEBROOT_SYNC="1"

so that when an assets is added to the native filesystem of one of the webservers it is synchronised to the other webservers

-------------------------------

#### APPLICATION WITH SUBSTANTIAL ASSETS OFFLOADED TO S3 AT AN APPLICATION LEVEL

If your application is expected to create a substantial amount of asset data such as a social network with users uploading images and videos to your server(s) you can offload your assets to an S3 storage system at an application level. You can do that level plugin such as [Amazon S3 Offload](https://extensions.joomla.org/extension/amazon-s3-filesystem/) for Joomla or [Offload to S3](https://en-gb.wordpress.org/plugins/amazon-s3-and-cloudfront/) for wordpress. Using one of these plugins will mean that your application has the full capacity of an S3 bucket. For example, "Amazon S3 buckets have virtually unlimited total storage capacity, allowing users to store an infinite amount of data, ranging from gigabytes to exabytes" providing of course you have got very deep pockets. Some CMS systems won't provide support for such application level solutions so in those cases this solution to asset offloading might not be possible. 

-----------------------------------

#### APPLICATION WITH SUBSTANTIAL ASSETS OFFLOADED TO S3 IMPLICIT TO THIS TOOLKIT WITHOUT USING A REVERSE PROXY MACHINE

To do this you need to set:

>     PERSIST_ASSETS_TO_DATASTORE="1"

If your application doesn't have the ability to use a plugin to offload assets to s3 buckets then you can mount an S3 bucket using our toolkit directly into the "assets" directory of your application's webroot(s). The options that are available for doing this are "S3fs", "goofys", "rclone" and "geesefs". You might want to spend some time tuning your mount tool of choice if you decide to use one of these so it has optimal caching and so on. You can use these S3 mount tools bidirectionally in order to write assets to s3 via your local looking filesystem or to fetch assets from s3 via your local looking filesystem. This will likely have a heavy footprint. If you don't want to use your own reverse proxy machnes but are willing to use Cloudflare for your DNS (for example) you can use the S3 mount tools listed above to upload the assets to S3 and then use Cloudflare to redirect read requests for assets to s3 rather than reading the assets from the file system of your VPS via through the mount system and the mount tool such as rclone. To see how this might be possible to do please read [Cloudflare redirect assets to S3](https://developers.cloudflare.com/rules/cloud-connector/examples/route-images-to-s3/). You might have privacy issues if you use cloudflare becuase your bucket will need to only be accessible by cloudflare IP addresses and you can find out below how I do this for our custom reverse proxies using S3 policies if you wanted to use this technique. 

#### APPLICATION WITH SUBSTANTIAL ASSETS OFFLOADED TO S3 IMPLICIT TO THIS TOOLKIT USING REVERSE PROXY MACHINE(S)

In this case you need to set 

>     PERSIST_ASSETS_TO_DATASTORE="2"

It's possible that if you are deploying your own custom reverse proxies you can redirect requests your upstream reverse proxy can redirect read requests for assets and so on directly to the S3 bucket that your application is uploading user updates to through the S3 mount tools. To do this a policy is set to limit acccess to the bucket by referrer (I believe s3cmd can do this but s5cmd can't and so I force the use of s3cmd to do this within the toolkit) for your assets bucket that resticts access to only the a unique referer ID which I set using the "SECRET_IDENTIFIER" set in your template or StackScript of your reverse proxies, and as I mentioned above the same thing applies with cloudflare as well unless, of course, you are happy with your asset buckets being world readable. To set a policy for your bucket(s) to restrict access to particular IP addresses (cloudflare IP addresses, for example) you will need to have a policy.json file similar to how I do it for the custom reverse proxies.

1. The bucket needs to be set to private in the overall ACL for the bucket

2. A policy needs to be geerated and set similar to the following:

>     {
>         "Version": "2012-10-17",
>         "Statement": [
>             {
>                 "Effect": "Allow",
>                 "Principal": "*",
>                 "Action": "s3:*",
>                 "Resource": [
>                     "arn:aws:s3:::<bucket-name>",
>                     "arn:aws:s3:::<bucket-name>/*"
>                 ],
>           "Condition": {
>             "StringLike": {
>               "aws:Referer": [
>                 "https://<unique-website_url>"
>               ]
>             }
>             }
>            }
>         ]
>     }

**NOTE:** the unique-website-url is constructed from "SECRET_IDENTIFIER" and you need to retain the value of that SECRET_IDENTIFIER to be able to access these buckets in this configuration. If you take the system offline and then redeploy it if you haven't retained and provided the same SECRET_IDENTIFIER as you set for the first deployment, the assets won't be accessible. 

----------------------------------------------

#### FULL WORKFLOW/EXPLANATION FOR HOW TO MOUNT ASSETS FROM S3 USING THIS TOOLKIT

**Part one** of how to use S3 as your asset store

In the application descriptor for your application you can specify directories within your application webroot that you want to be mounted from an S3 bucket across mulitple servers so that for the assets for your application you have greater capaciity.
In order to use the S3 datastore bucket mounted as a particular directory within your webroot(s) you can set it up as follows:

1. You  need to set these qualities in your application's descriptor on the build machine repository

Using joomla as an example the joomla descriptor file is located at:

>     ${BUILD_HOME}/application/cms/joomla/descriptor.dat

And so if I want to mount the images directory as a S3 backed mount point in my application webroot I can set:

>     ASSETS_OUTSIDE_WEBROOT:yes
>     WEBROOT_ASSET_DIRECTORIES:images
>     LINK_INSIDE_WEBROOT:images

In joomla the "assets" directory is the "images" directory  do the above examples are how you set the images directory to be mounted as an s3 bucket.

2. You will need to make sure that a S3 datastore mount tool is configured in the 

>     ${BUILD_HOME}/configuration/software.dat

>     DATASTOREMOUNTTOOL:rclone:binary
>     #DATASTOREMOUNTTOOL:s3fs:source
>     #DATASTOREMOUNTTOOL:goof:binary
>     #DATASTOREMOUNTTOOL:geesefs:source

This will set the datastore mount tool to rclone which means that the system will use rclone to mount the S3 datastore bucket into your application webroot. 

3. There's one other setting you need to set in order to have your defined directory mounted from the S3 datastore and that is

>     PERSIST_ASSETS_TO_DATASTORE="1" (if you are not using custom reverse proxies)

or

>     PERSIST_ASSETS_TO_DATASTORE="2" (if you are using custom reverse proxies)

in your template or StackScript.

The settings I have set here will mount the /var/www/html/joomla/images directory from S3 at /var/www/html/images using the rclone datastore mount tool

**Part two** means that you need to be aware of in setting your application to use S3 to mount assets into the webroot is that you need to have an application flow between virgin, baselined repository code and temporal datastore backups.

So, here's the workflow to go from nothing to a temporal backup based full deployment

1. Deploy a virgin copy of your CMS of choice and make your custom application
2. Once your custom application is ready make a baseline of the application
3. Deploy the baselined application (without reverse proxies) with the settings I showed you above in the part one and that will automatically prepare the images directory and upload the application assets to S3 with the assets that are in the baseline repository
4. Either allow enough time for the system to make a temporal backup or make a temporal backup manually.
5. You can then take your baselined application offline (shutdown your servers) and you can make a full scale deployment (the images directory will be absent from your temporal backup and will be expected to be provided from the S3 bucket that the system generated when you deployed the baselined application). And then using the same settings for WEBROOT_ASSET_DIRECTORIES, DATASTOREMOUNTTOOL and PERSIST_ASSETS_TO_DATASTORE that were set for the baseline and when you make the deployment from a temporal backup the images directory will be mounted from S3.

So, in summary

Make a baseline of your application  
Deploy your baseline setup to persist its specified assets to the datastore  
Take the baseline application offline once a temporal backup has been generated/created  
Deploy the full temporal deployment of your application expecting the assets to be mounted from the S3 datastore using reverse proxies if you want to.  
-----------------------------

#### ELASTIC FILE SYSTEM SUPPORT

I didn't put in support for mounted elastic filesystems because 

a) Its pricey to do so compared to S3   
b) The elastic filesystem support that is available for the cloudhosts I use is tied to only one region   
c) Not all the cloudhosts I use even have elastic filesystem offerings.   

You are welcome to make updates to support elastic filesystems if you want to or if you have a need for them. If all of the providers I used supported elastic filesystems then I would probably add in support for mounted elastic filesystems in addition to the (probably inferior but cheaper way I operate here using S3). 


When it comes to "application assets" there are various options that are provided for. You will have to decide which solution is the best fit for you and your requirements because based on what you are doing it could be possible that any of these solutions will be your solution of choice. 
