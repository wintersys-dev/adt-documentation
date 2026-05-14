#### GENERATING A LINODE STACKSCRIPT

For Linode it's possible to use "StackScripts" to make your deployment. These can be easier to manage that "cloud-init" scripts, but, you have to have good knowledge of how the tookit functions to be able to make a successul deployment using a StackScript. What you will have to do is generate the StackScript which will only be useful on Linode and you can do this as follows. 

1. Perform a test expedited build using the parameters you require for your deployment. This will mean that you have a pre-populated template which deploys succcessfully which can be used as the basis for generating the userdata or stack script.  

2. On the same machine that you performed (and tested) the Expedited build run the script:  

>     ${BUILD_HOME}/helpers/templates/GenerateOverrideTemplate.sh
    
You should select the provider and template number (1, 2 or 3) that you wish to generate from  
    
Once the script has completed successsfully it will be available in  
    
>     ${BUILD_HOME}/overridescripts  
    
3. To generate the Stackscript, run
    
>     ${BUILD_HOME}/helpers/templates/GenerateHardcoreUserDataScript.sh stack  
    
This will write the User Data / Stack Script to   
    
>     ${BUILD_HOME}/userdatascripts/<name you gave>  
    
 4. The script

>     ${BUILD_HOME}/userdatascripts/<name you gave>

You then need to copy the StackScript you have generated to your Linode account and you can then use it to make a deployment.  
The advice is to generate a library of StackScripts that you can use each of which has a different function or configuration so that you can select the StackScript with appropriate settings based on what your requirments are. For example you might have a StackScript which deploys small capacity servers using cloudflare for the DNS using one StackScript in your library and you might deploy large capacity servers using Linode for the DNS system with another StackScript in your library.   


can then be used as a Stackscript on Linode. My Stackscript for the ADT has ID 635271 you are welcome to use that. It is called "AgileDeploymentToolkitDemos" it is publically available and I use it for my "Quick Demo" example deployments. 
