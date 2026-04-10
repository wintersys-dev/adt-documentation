BUILDING FROM SNAPSHOTS

**NOTE 1: Building servers from snapshots is only possible using the Expedited build method the tookit isn't setup for snapshot builds in hardcore mode**  

**NOTE 2: You must always make sure that the configuration of the template you are using to deploy from snapshots is consistent with the snapshots you are deploying. For example if your snapshot is built for using apache and your template is configured to use NGINX, you will obviously have issues**  

There can be some performance (meaning quicker deployments of, in particular webservers spawned during autoscaling events) if snapshots are pre-prepared and our server instances provisioned using snapshots so I thought I would go over how to use snapshots.

If you deploy your server fleet as you normally would so that you have an autoscaler, webserver and database machine each, then you can make snapshots of those machines, take them offline and then deploy new server machines (including newly provisioned webservers due to autoscaling  events) from snapshots. 

1. Deploy your server fleet
2. Once your server fleet is deployed and only, on you build machine run the script for each machine type

>     ${BUILD_HOME}/helpers/ObtainWholeMachineSnapshot.sh


and this will generate snapshots of your machines that you provisioned in step 1. The unique identifiers of your snasphots will be stored in 

>     ${BUILD_HOME}/runtimedata/wholemachinebackups/${WEBSITE_URL}/snapshots/snapshot_ids.dat

and the database credentials for your snapshots in 

>     ${BUILD_HOME}/runtimedata/wholemachinebackups/${WEBSITE_URL}/snapshots/db_credentials.dat

3. Check that all of your snapshots have been generated using the GUI system of your cloudhost and then take your original servers down. 

4. In your template set:

>     BUILD_FROM_SNAPSHOT="1"

5. Re-run the build process. This time when a server is provisioned the snapshot ids will be obtained from 

>     ${BUILD_HOME}/runtimedata/wholemachinebackups/${WEBSITE_URL}/snapshots/snapshot_ids.dat

and the database credentials from 

>     ${BUILD_HOME}/runtimedata/wholemachinebackups/${WEBSITE_URL}/snapshots/db_credentials.dat

And the machines will be provisioned based on the snapshots and database credentials that are stored there.

6. The snapshot id for the webserver snapshot image is passed to the autoscaler machine as part of the build process and is then used because

>     BUILD_FROM_SNAPSHOT="1"

to provision new webservers by the autoscaler in response to a "scale up" event. 

NOTE: snapshots can only be used when you are deploying using a temporal backup and also, the actual webroot and database are reinstalled based on the webroot archive and database dump that you have stored in your datastore. This means that a snapshot image can be weeks old but it will still have the application and database of your latest backup which might be less than an hour old when you make your build using snapshots. Furthermore, presuming that you are running your server fleet for more than 1 day, the software on the machines will be updated to the latest version overnight on the first night of full provisioning. If you  need your software updated more quickly you can do so manually. 
