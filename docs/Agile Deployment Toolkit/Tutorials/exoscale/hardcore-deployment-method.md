**GENERATING A USERDATA SCRIPT FOR THE HARDCORE DEPLOYMENY BUILD STYLE**

To get a "hardcore" userdata script that you can use for multiple deployments what you need will need a fully configured template that the user data script can be built from. To have a fully configured template, you can follow the appropriate tutorial below depending on whether you want your userdata script to deploy a virgin CMS, a baselined application or a deployment based on a temporal backup in your datastore.

[Virgin CMS Installation](./expedited-virgin-joomla.md)   
[Baseline CMS Installation](./expedited-baseline-joomla.md)  
[Temporal CMS Installation](./expedited-temporal-joomla.md)

Once you have made a deployment using one of these tutorials then you can be confident that the template associated with the deployment is valid and knowing that you have a valid template you can go ahead and create a hardcore build userdata script. 

Virgin class builds are associated with template1 (exoscale1.tmpl), baseline builds are associated with template 2 (exoscale2.tmpl) and temporal builds are associated with template 3 (exoscale3.tmpl)

In this example, we will use template 1 assuming that we have followed the Vigin CMS installation tutorial.  

>     /bin/mkdir ${BUILD_HOME}/adt-build-machine-scripts/overridescripts/
>     /bin/cp ${BUILD_HOME}/adt-build-machine-scripts/templatedconfigurations/templates/exoscale/exoscale1.tmpl ${BUILD_HOME}/adt-build-machine-scripts/overridescripts/exoscale1override.tmpl  

Then you need to run the script:

>     cd helpers

>     ./GenerateHardcoreUserDataScript.sh

This will leave you with a script:

>     ${BUILD_HOME}/userdatascripts/${userdatascript}   

where ${userdatascript} is the descriptive name you gave when prompted.  

Have a look at the userdata script 

>     vi ${BUILD_HOME}/userdatascripts/${userdatascript} 

Now, substitute this userdata script that you have generated instead of the default initial script when you follow  [Build Machine Setup](./buildmachine.md). When you use your userdata script instead of the default init script, the build will proceed automatically with no need for interaction from you. Once you have a user data script you can use it for repeated deployments tweaky it as necessary for each deployment cycle.

<span style="color:red">**NOTE: remember to set SELECTED_TEMPLATE in your user data script when you are deploying your own hardcore style user data script </span>
