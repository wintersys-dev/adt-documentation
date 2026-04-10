There are various scenarios related to the dynamic assets such as images, text files and videos that your application generates. I will outline those scenarios here. First of all I will describe scenarios where elastic filesystems are not available for mount because the current provider either doesn't support them or you don't want to use them because they are more expensive than S3 solutions. 

Just to make a mental note that the best solution is to offload your assets to S3 at an application level using tools such as:

[amazon s3 filesystem](https://extensions.joomla.org/extension/amazon-s3-filesystem) for joomla

or

[offload media](https://wordpress.org/plugins/offload-media-cloud-storage) for wordpress

If, however such application level solutions are not available or you  don't want to use them for some reason then I provide a way of getting around this and still giving you S3 level capacity for your assets as I describe below. 

1. You have multiple webservers but your application doesn't generate many or even any dynamic assets

You can just use the standard webroot /var/www/html synchronised with the webroots of the other machines using the heavyweight file sync mechanism.

You can read about how to setup your webroot for heavyweight synchronisation here:

[heavyweight](https://www.wintersys-dev.uk/Agile%20Deployment%20Toolkit/Operations/FileSystemSyncing)

2. Your application generates a lot of dynamic assets:

You mount an S3 bucket for each of the various directories that you expect your application to produce assets into. This will involve setting up your template such as

>     PERSIST_ASSETS_TO_DATASTORE="1"
>     DIRECTORIES_TO_MOUNT="images:media/videos"

When you set these values in your template it will mount two separate buckets created in your datatstore as /var/www/images and /var/www/media/videos

When your application accesses these asset directories it will be accessing an bucket mounted from s3. The mount is made by using one of the following tools depending how you have configured "software.dat"

>     s3fs, goofys, geesefs, or rclone

You will then have a whole s3 bucket for each of these mount points and so you could have "n" buckets mounted into your webroot and have them all as their individual bucket giving you huge capacity. There is a performance penalty and it remains to be seen how ths functions under load but as a solution it exists right now although it is not commonly recommended to mount s3 buckets like this. It's possible you can fine tune the parameters of your choosen tool compared to what is in the default script to improve performance for your particular use case. 

Its not recommended for most scenarios but you might have an application usage scenario where you need enormous data capacity on a single mount point and performance is not so important and in that case you can use mhddfs in software.dat to merge multiple s3 buckets into a single mount point in your webroot. Under this scenario you could mount 4 or more full capacity s3 buckets as /var/www/html/images where if you had some crazy stuff going on in terms of the size of your application assets you would be able to accomodate it (if you have got deep pockets that is because object storage is not free at such a large scale). 

Again I am describing situations where elastic filesystems are not available which means you will need to turn on heavyweight filesyncing.

You can read about how to setup your webroot for heavyweight synchronisation here:

[heavyweight](https://www.wintersys-dev.uk/Agile%20Deployment%20Toolkit/Operations/FileSystemSyncing)

When you switch on the heavyweight filesync mechanism for your webroot the webroots of your webservers will be synchronised but the directories where the assets are that are mounted from s3 will be excluded from the webroot synchronisation process. This means that the data in the S3 buckets is the only source of your application assets. 

3. The final option is if elastic filesystem solutions are available for your provider and this is the neatest solution but it is also considerably more expensive than using an S3 backed solution and it has lower capacity than an s3 backed solution. When you use elastic filesystems you don't need to have any file synchronisation mechanism between webservers because each webserver mounts the same elastic filesystem directly as its entire webroot. Under this scenario you can use mdhhfs to merge multiple elastic filesystems into a single mount point and also you can exclude asset directories from backups (so that you backups don't get huge) by

>     PERSIST_ASSETS_TO_DATASTORE="0"
>     DIRECTORIES_TO_MOUNT="images:media/videos"

This will exclude the directories "/var/www/html/images" and "/var/www/html/media/videos" from the backup process
