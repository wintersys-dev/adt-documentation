Depending on your application type you might have different requirements for your webservers in terms of how you configure your application's webroot. It's quite possible in different scenarios down the line you might have applications with different needs, for example you might have an application with no dynamic assets being uploaded and a relatively small code base in which case the first of the solutions I am about to describe will likely be the best fit solution. You might have another application with terrabytes of data to access (at a reasonable price) with speed not being such an important factor for you in which case solution 1 will not be suitable and you will want one of the other solutions. If you have a lot of assets and access speed is important then you and you are flush with cash you might want to use an elastic filesystem (which I don't currently support, 2025, but, I intend to add support for in 2026). So, you need to do an analysis of which of the provided solutions is the best fit for your application before you make a deployed  use of it. So, let me outline some various scenarios in a bit more detail:

1. The native filesystem of your webserver without any services to embellish it. This would be how you could arrange things if your application doesn't produce dynamic assets through user uploads and so on and it has a relatively small code base. If you have more than one webserver you will likely want to switch on **SYNC_WEBROOTS** to synchronise the webroots of your different webservers when any changes are made to any one of them such as application updates or new application installations and so on.

2. Offload your dynamic assets at an application level. CMS systems such as Joomla provide solutions or plugins to offload dynamic assets to an S3 backed storage solution, for example: [amazon s3 for joomla](https://extensions.joomla.org/extension/amazon-s3-filesystem/). I don't know whether these solutions work with non amazon providers, you will need to look into it for your particular provider, but, if this is the prefered solution for offloading your dynamic assets because it reduces the load on your webserver as the webserver isn't touched when these assets are being accessed. In some situations ([community builder](https://www.joomlapolis.com/forum/professional-members-support/246825-cdns)) for example, offloading assets at an application level are not possible at the moment  and so I made the effort to build out a solution (described next) using tools such as rclone, s3fs, geesefs and goofys to mount S3 buckets to the webservers directly and I describe that next. 

3. Bear in mind that this solution will increase the load on your webservers compared with solution 2 above. If you want to have a very large data capacity available to you you can mount various buckets which are automatically created according to a defined nomenclature by the ADT when you set some parameters in your template as described [here](https://www.wintersys-dev.uk/Agile%20Deployment%20Toolkit/Operations/WebrootConfigurationWorkflow/). So, you can set as many directories as you want here so your application might want one s3 bucket backed directory for images one s3 bucket backed directory for data and one s3 bucket backed directory for videos and because each of these directories is a mount of an S3 bucket, each directory has the capacity of a whole bucket (whatever the data limits are for your chosen provider).

4. And so if even that is not enough you can have up to 9 buckets all merged together as a single shared mount point. So you could have 6 s3 buckets all merged together as a single s3 mount point which gives you 6 times whatever the capacity limit of a single bucket is for your current provider at one mount point. Let me know how you get on if you try this.

>     /var/www/images1,/var/www/images2,/var/www/images3,/var/www/images4/var/www/images5,/var/www/images6

as a single shared mount to

>     /var/www/html/images

This is easy to setup all you have to do is put the following into your template:

>     DIRECTORIES_TO_MOUNT="merge=images6"

And so what that will do is it will create 6 buckets if they don't exist mount them to your webservers file system as separate mount points **/var/www/images1, /var/www/images2**, for example, and then merge them into a single shared mount point **/var/www/html/images**

So, there's several layers to that and being backed by S3 as well as having merge operations performed it won't be as fast as using a native filesystem but the advantage you get here is extremely large shared capacity on your webservers as cheaply as you can get it because it is S3 based rather than native. 

So, you might have your setup as following if you want a shared mount

>     PERSIST_ASSETS_TO_DATASTORE="1"
>     DIRECTORIES_TO_MOUNT="merge=images6:media/data"

And that will give you 7 buckets mounted to two mount points (**/var/www/images** and **/var/www/media/data**). 

The reason I offer this as a solution is two fold: cost (S3 storage is cheaper than block storage) and secondly (at the time of writing) elastic filesystems have become available for two of the cloudhosts that I support which I am going to integrate support for in due course (its 2025 right now and I intend to add support for them in 2026). Elastic filesystems are more expensive however so if you are on a budget you might prefer to use S3 backed storage instead of the elastic file systems that are being offered now. The elastic filesystems I have seen at the moment are from [vultr](https://www.vultr.com/products/file-system) and [digital ocean](https://www.digitalocean.com/products/storage/network-file-storage) if you see wintersys documentation for these products I have implemented support for elastic filesystems for those two providers, if linode and exoscale add support for elastic filesystems I will integrate those into my toolkit as well. 
