You can use the scripts against the template of your choice:

>     ${BUILD_HOME}/helpers/GenerateOverrideTemplate.sh
>     ${BUILD_HOME}/helpers/GenerateHardcoreUserDataScript.sh

to generate hardcore userdata scripts which you can simply paste into the cloud-init area of a newly provisioned machine in order to make a deployment
correspoding to how your userdata script is configured. 

You could spend some time setting up hardcore userdata scripts for yourself such that you have a library of userdata scripts for different scenarios and 
store them on your laptop. You could, for example, generate a hardcore userdata script which deploys a virgin joomla installation for a particular version 
of joomla to Exoscale with cloudflare DNS which you can tweek and reuse every time you want to deploy a fresh copy of joomla to Exoscale. Similar for wordpress, drupal and moodle. You can also have hardcore userdata scripts in your library which deploy particular baselines or particular temporal backups
and so on.   

If you take the time upfront this will be the fastest way to make new deployments but it is more hardcore because any errors in your userdata script
will not be treated gently whereas if you use the slightly more involved expedited method there is a lot more sanity checking than there is with 
using a hardcore userdata script. 

The cautious way to proceed is to make an expedited deployment for the configuration you are interested in and then run the GenerateHardcoreUserData.sh
script against the template that has been successfully deployed and then you can be more confident that when you deploy your hardcore userdata script
that it won't have typos and whatnot in it. 
