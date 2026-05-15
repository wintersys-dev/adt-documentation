#### Arrangement 1

If you just have a single webserver with not many user generated assets then you can just have a webroot which is the disk of the VPS server that your webserver runs on.

```SYNC_WEBROOTS="0"```  
```WEBROOT_ASSET_DIRECTORIEST=""```  (this is stored in ${BUILD_HOME}/application/cms/${APPLICATION}/application.dat)
```PERSIST_ASSETS_TO_DATASTORE="0"```  

#### Arrangement 2

You can have arrangment 1 on multiple webservers and have the webroots synchronised. To switch on webroot synchronisation which means that updates to any of the webroots are synchronised in short order to all the other webroots in your system by having the following relevant settings in your template

```SYNC_WEBROOTS="1"```  
```WEBROOT_ASSET_DIRECTORIEST=""```  (this is stored in ${BUILD_HOME}/application/cms/${APPLICATION}/application.dat)
```PERSIST_ASSETS_TO_DATASTORE="0"```  

#### Arrangment 3

You have a lot of user generated assets and multiple webroots/webserver machines in your architecture. To run with that configuration you need to have the following settings. If your application is a joomla application then you will likely have user assets generated in the "images" directory and so that configuration will be arranged like this. When assets are peristed to the datastore that means that they are written to the object store and mounted into your webroot using a tool like rclone. This is not ideal but it is a solution. Your first preference should be to offload your user generated assets to an object store at an application level using a plugin or an extension of some sort but if that is not possible you can get by like this in a lot of cases. 

```SYNC_WEBROOTS="1"```  
```WEBROOT_ASSET_DIRECTORIES="images"```  (this is stored in >.    ${BUILD_HOME}/application/cms/${APPLICATION}/application.dat)
```PERSIST_ASSETS_TO_DATASTORE="1"```  

 
