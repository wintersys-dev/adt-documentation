This article just contains a workflow overview of the two different build types for this toolkit

**EXPEDITED BUILD TYPE:**

An expedited build type will involve the following steps to give you a high level overview  

1. Configure a build machine according to your cloudhost provider type:
   
   [digitalocean](../Tutorials/digitalocean/buildmachine.md)  
  
   [exoscale](../Tutorials/exoscale/buildmachine.md)  
  
   [linode](../Tutorials/linode/buildmachine.md)  
  
   [vultr](../Tutorials/vultr/buildmachine.md)  
 

2. Assuming that you have followed the correct set of instructions in 1. you should have ssh'd onto your build machine and you should then issue the following command

>     sudo su

and enter the password "BUILDMACHINE_PASSWORD" that you configured in the user data script that you configured during stage 1

4. You can then change directory to the adt-build-machine-scripts home directory which should be able to see by issuing a "ls" comamnd.
5. What you then need to do is go and configure your template. The template is located at

>     ${BUILD_HOME}/templatedconfigurations/templates/${CLOUDHOST}

You can choose template 1, 2 or 3 depending on whether you are deploying a virgin, baseline or temporal type of build. You will then need to configure your chosen template according to your deployment needs and in accordance with **[The Specification](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/specifications/specification.md)**. If you look through the [tutorials](../Tutorials/TutorialsMenu.md) for your provider you can see more detail on how to configure your template (pay particular attention to any fields in the template maked MANDATORY because you must provide live values for those in all circumstances)

6. Once your template is configured you can run the script "ExpeditedAgileDeploymentToolkit.sh" by issuing the commands

>      cd /home/${BUILDMACHINE_USER}/adt-build-machine-scripts
>      /bin/sh ./ExpeditedAgileDeploymentToolkit.sh

The script will run and there will be some questions to answer. If you are an expert and you want to avoid answering questions you can use the parameters to the script to avoid the interaction:

>     CLOUDHOST, BUILDOS, SELECTED_TEMPLATE, BUILD_IDENTIFIER
>     for example: /bin/sh ./ExpeditedAgileDeploymentToolkit.sh exoscale ubuntu 2 testbuild

----------------------------

**HARDCORE BUILD TYPE**

The hardcore build type is some extra fuss to begin with but if you are an expert then once you have a "user data" script generated you could simply keep a copy of it on your laptop and "tweak" it per deployment using the **[The Specification](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/templatedconfigurations/specification.md)** for guidance and a deployment would be no more effort than taking a copy of your script and pasting it into the userdata area of a new build machine and that would be it. It can be faster but its not faster if you make configuration mistakes that the expedited build process will hopefully warn you about before the build starts in ernest. If you have a "well worn (deployed multiple times)" hardcore user data script then you should be relatively confident that you have sane parameter values set in it. What I am saying, basically I am just saying that using the hardcore build type could be quicker and easier for you if you are an expert in the ADT. Below, then, are the steps that you need to generate a user data script for a hardcore build. 

During a **hardcore build**, you need to

1. On your local laptop clone the build client scripts for example (or from your fork):  

>     cd CLONE_DIR
>     /usr/bin/git clone https://github.com/wintersys-dev/adt-build-machine-scripts.git

2. Still on your local laptop set up your template, if you are using the hardcore build method you should be familiar with how to do that by now but if not you can refer to the [tutorials](../Tutorials/TutorialsMenu.md)

>     ${BUILD_HOME}/templatedconfigurations/templates/${CLOUDHOST} 

4. Return to the directory

>      CLONE_DIR/adt-build-machine-scripts

5. You now need to generate an override template that you will be using in the hardcore build method

>     /bin/sh ./helpers/GenerateOverrideTemplate.sh and answer the questions as it asks them  

6. You now need to generate a user data script which you can do:

>     /bin/sh ./helpers/GenerateHardcoreUserDataScript.sh and answer the questions as it asks them  

7. Then you need to use your user data script that you generated in step 6 in the as the cloud-init script of a VPS server with your chosen VPS provider

8. >     cd ${BUILD_HOME}/userdatascripts

and copy the script that has been generated and paste it into the user-data of a new VPS machine that will then become your build-machine.  

NOTE: I called this technique "Hardcore" because you need to have a lot of understanding of the toolkit to make it work properly. If you are deploying from a StackScript on Linode or a cloud-init script on any provider, you might set the SSL certificate provider to ZEROSSL but unless you realise that you need to set the SSL CLIENT to "acme" in the software.dat file if it is set to "lego" for example you will configure it all and then the build will fail. Possibly the best way to use hardcore build styles if if a) You have detailed knowledge of the toolkit or b) you spend some time figuring out what you want your configuration to be and then maybe having librarires of StackScipts or cloud-init scripts that you can deploy in short order repeatedly. It is recommended, therefore, to use the expedited build method (in other words, set up a single build machine and make repeated deployments from it) unless you have a very thorough understanding of the interactions within the ADT. The expedited build method will apply various (non exhaustive) sanity checks before the build commences making it less likely there will be configuration issues. Of course, the trick could be to perform and expedited build, crib, the values that "work" from your template and then use those values to perform hardcore builds. 
