#### BUILD METHODS

There are two basic build modes that can be used to deploy a website using this toolkit. One is the expedited build method and the other is the hardcore build method. The basic difference is that the Expedited build method is interactive, has improved sanity checking and is initiated on a prepared build machine from the command line. An expedited build will also tend to be on a build machine that will make multiple or repeated deployments where as a hardcore build will use a build machine (most likely) for only one build. A hardcore build is basically intended to be initiated from using cloud-init on a ubuntu or debian machine and have that cloud-init script run to completion at which point  the website will be online. The reason I call it hardcore is beccause you have to have a very good understanding of how to configure the parameters of the cloud init script (or the StackScript on Linode) because if your parameters are "wrong" in any way then you will go through the whole process of initiating a build process and have it fail ungracefully at some point in its build cycle because of incorrect parameter configuration. I personally prefer the hardcore build method but I wrote the toolkit so I know its function but its recommended for you to use the expedited build process until you are more familiar with how things work with this software. Another thing I would say is that you  could have a library of cloud-init  scripts configured for different deployment scenarios so you could have a cloud-init  script set up and validated to deploy a virgin joomla installation to Linode and another one set for a virgin deployment of Wordpress on Exoscale and so on for any combination of configuration that your heart desires. If you have a library of pre-tested cloud-init scripts set up for your needs then you can use them with confidence to make different types of deployments. 

Some detail about the different build types

##### EXPEDITED BUILD TYPE:

An expedited build type will involve the following steps to give you a high level overview  

1. Configure a build machine according to your cloudhost provider type:
   
   [digitalocean](../Tutorials/digitalocean/build-machine.md)  
  
   [exoscale](../Tutorials/exoscale/build-machine.md)  
  
   [linode](../Tutorials/linode/build-machine.md)  
  
   [vultr](../Tutorials/vultr/build-machine.md)  
 

2. Assuming that you have followed the correct set of instructions in 1. you should have ssh'd onto your build machine and you should then issue the following command

>     sudo su

and enter the password "BUILDMACHINE_PASSWORD" that you configured in the cloud-init (user data) script that you configured during stage 1

4. You can then change directory to the adt-build-machine-scripts home directory which should be able to see by issuing a "ls" comamnd.
5. What you then need to do is go and configure your template. The template is located at

>     ${BUILD_HOME}/templatedconfigurations/templates/${CLOUDHOST}

You can choose template 1, 2 or 3 depending on whether you are deploying a virgin (1), baseline (2) or temporal (3) type of build. You will then need to configure your chosen template according to your deployment needs and in accordance with **[The Specification](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/specifications/specification.md)**. If you look through the [tutorials](../Tutorials/TutorialsMenu.md) for your provider you can see more detail on how to configure your template (pay particular attention to any fields in the template maked MANDATORY because you must provide live values for those in all circumstances)

6. Once your template is configured you can run the script "ExpeditedAgileDeploymentToolkit.sh" by issuing the commands

>      cd /home/${BUILDMACHINE_USER}/adt-build-machine-scripts
>      /bin/sh ./ExpeditedAgileDeploymentToolkit.sh

The script will run and there will be some questions to answer. If you are averse to being questioned and you want to avoid answering questions you can use the parameters to the script to avoid the interaction:

>     CLOUDHOST, BUILDOS, SELECTED_TEMPLATE, BUILD_IDENTIFIER
>     for example: /bin/sh ./ExpeditedAgileDeploymentToolkit.sh exoscale ubuntu 2 testbuild

----------------------------

**HARDCORE BUILD TYPE**

The hardcore build type is some extra fuss to begin with but if you are an expert then once you have a "user data" or "cloud init" script generated you could simply keep a copy of it on your laptop and "tweak" it per deployment using the **[The Specification](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/templatedconfigurations/specification.md)** for guidance and a deployment would be no more effort than taking a copy of your script and pasting it into the userdata area of a new build machine and that would be it. It can be faster but its not faster if you make configuration mistakes that the expedited build process will hopefully warn you about before the build starts in ernest. If you have a "well worn (deployed multiple times)" hardcore user data script then you should be relatively confident that you have sane parameter values set in it. I am just saying that using the hardcore build type could be quicker and easier for you if you are an expert in the ADT but slower and considerably more jip if you are not. Below, then, are the steps that you need to generate a user data script for a hardcore build. 

During a **hardcore build**, you need to

1. On your local laptop clone the build client scripts for example (or from your fork):  

>     cd CLONE_DIR
>     /usr/bin/git clone https://github.com/wintersys-dev/adt-build-machine-scripts.git

2. Still on your local laptop set up your template, if you are using the hardcore build method you should be familiar with how to do that by now but if not you can refer to the [tutorials](../Tutorials/TutorialsMenu.md)

>     ${BUILD_HOME}/templatedconfigurations/templates/${CLOUDHOST} 

4. Return to the directory

>      CLONE_DIR/adt-build-machine-scripts

5. You now need to generate an override template that you will be using in the hardcore build method

>     /bin/sh ./helpers/templates/GenerateOverrideTemplate.sh and answer the questions as it asks them  

6. You now need to generate a user data script which you can do:

>     /bin/sh ./helpers/templates/GenerateHardcoreUserDataScript.sh and answer the questions as it asks them  

7. Then you need to use your user data script that you generated in step 6 in the as the cloud-init script of a VPS server with your chosen VPS provider

8. >     cd ${BUILD_HOME}/userdatascripts

and copy the script that has been generated and paste it into the user-data of a new VPS machine that will then become your build-machine.  

There is a StackScript available at [StackScript](https://cloud.linode.com/stackscripts/635271) that you can use for a hardcore build on Linode that might be less fuss than preparing a whole cloud-init script. Again, please note, once you have set up a cloud-init or userdata script that you can reuse with confidence that is when the hardcore build process has the potential to be much faster. 
