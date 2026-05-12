

I didn't put in support for mounted elastic filesystems because 

a) its pricey to do so compared to S3 
b) the elastic filesystem support that is available for the cloudhosts I use is tied to only one region 
3) Not all the cloudhosts I use even have elastic filesystem offerings. 

You are welcome to make updates to support elastic filesystems if you want to or if you have a need for them. If all of the providers I used supported elastic filesystems then I would probably add in support for mounted elastic filesystems in addition to the (probably inferior but cheaper way I operate here using S3). 

### APPLICATION ASSETS STORAGE

When it comes to "application assets" there are various options that are provided for. You will have to decide which solution is the best fit for you and your requirements because based on what you are doing it could be possible that any of these solutions will be your solution of choice. 

---------------------------------

#### AN APPLICATION WITH NOT MANY ASSETS 

Your application doesn't have or expect to have many assets, for example if its a personal blog you might not ever create more than a couple of GB of asset data so you can simply use the webserver's native filesystem to store your assets without any hassle at all. 

-------------------------------

#### APPLICATION WITH SUBSTANTIAL ASSETS OFFLOADED TO S3 AT AN APPLICATION LEVEL

Your application is expected to create a substantial amount of asset data such as a social network with users uploading images and videos to your server. Your first choice in this case should be to offload your application assets to an S3 storage system. You can do that using an application level plugin such as [Amazon S3 Offload](https://extensions.joomla.org/extension/amazon-s3-filesystem/) or [Offload to S3](https://en-gb.wordpress.org/plugins/amazon-s3-and-cloudfront/). Using one of these plugins will mean that your application has the full capacity of an S3 bucket. "Amazon S3 buckets have virtually unlimited total storage capacity, allowing users to store an infinite amount of data, ranging from gigabytes to exabytes" providing of course you have got very deep pockets. 

-----------------------------------

#### APPLICATION WITH SUBSTANTIAL ASSETS OFFLOADED TO S3 IMPLICIT TO THIS TOOLKIT

If your application doesn't have the ability to use a plugin to offload assets to s3 buckets then you can begrudgingly mount an S3 bucket using our toolkit directly into the "assets" directory of your application. The options that are available for doing this are "S3fs", "goofys", "rclone" and "geesefs". You might want to spend some time tuning your mount tool of choice if you decide to use one of these so it has optimal caching and so on. You can use these S3 mount tools bidirectionally in order to write assets to s3 via your local looking filesystem or to fetch assets from s3 via your local looking filesystem. This will likely have a heavy footprint. But if you are using cloudflare (for example) you can use the S3 mount tools listed above to upload the assets to S3 and then you might be able to use cloudflare to redirect read requests for assets to s3 rather than reading the assets via the mount system and through the mount tool such as rclone. To see how this might be possible to do please read [Cloudflare redirect assets to S3](https://developers.cloudflare.com/rules/cloud-connector/examples/route-images-to-s3/). Its possible that if you are deploying reverse proxies you can redirect requests for images and so on directly to the S3 bucket that your application is uploading user updates to through the S3 mount tools.

##### WORKFLOW/EXPLANATION FOR HOW TO MOUNT ASSETS FROM S3 USING THIS TOOLKIT

Part one of how to use S3 as your asset store

In the application descriptor for your application you can specify directories within your application webroot that you want to be mounted from an S3 bucket across mulitple servers so that for the assets for your application you have greater capaciity.
This will have some performance implications but should still be usable for most situations.
In order to use the S3 datastore bucket mounted as a particular directory you  can set it up as follows:

1. You  need to set these qualities in your application's descriptor on the build machine repository

Using joomla as an example this descriptor file is located at:

>     ${BUILD_HOME}/application/cms/joomla/descriptor.dat

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

>     PERSIST_ASSETS_TO_DATASTORE="1"

in your template or StackScript.

The settings I have set here will mount the /var/www/html/joomla/images directory from S3 using the rclone datastore mount tool

The second part that you need to be aware of in setting your application to use S3 to mount assets into the webroot is that you need to have an application flow between virgin, baselined repsitory code and temporal datastore backups.

So, here's the workflow to go from nothing to a temporal backup based full deployment

1. Deploy a virgin copy of your CMS of choice and make your custom application
2. Once your custom application is ready make a baseline of the application
3. Deploy the baselined application with the settings I showed you above in the first part and that will prepare the images directory with the assets that are in the baseline repository
4. Either allow enough time for the system to make a temporal backup or make a temporal backup manually.
5. You can then take your baselined application offline (shutdown your servers) and you can make a full scale deployment (the images directory will be absent from your temporal backup and will be expected to be provided from the S3 bucket that the system generated when you deployed the baselined application). And then using the same settings for WEBROOT_ASSET_DIRECTORIES, DATASTOREMOUNTTOOL and PERSIST_ASSETS_TO_DATASTORE that were set above and for the baseline and when you make the deployment from a temporal backup the images directory will be mounted from S3. 

So, in summary

Make a baseline of your application
Deploy your baseline setup to persist its specified assets to the datastore
Take the baseline application offline once a temporal backup has been generated/created
Deploy the full temporal deployment of your application expecting the assets to be mounted from the S3 datastore.
