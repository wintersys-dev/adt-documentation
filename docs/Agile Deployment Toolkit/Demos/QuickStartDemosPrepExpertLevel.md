### QUICK-START DEMOS (EXPERT STYLE) 

#### TLDR;

You need

1. The public key of your linux laptop
2. The IP address of your laptop
3. The username and email address of your Linode account
4. A full access PAT (Personal Access Token) for your Linode Account
5. Full access Access Key and Secret Key for your Object Storage
6. A Domain name with a DNS provider with its name servers set to: NS1.LINODE.COM  NS2.LINODE.COM  NS3.LINODE.COM  NS4.LINODE.COM  NS5.LINODE.COM
7. A VPC (adt-vpc) with subnet range 10.0.1.0/24 for the region you are deploying to
8. A firewall (adt-build-machine) for your build machine (blocking all access except to port 1035 from the IP address gathered in step 2 above)
9. The StackScript () configured with the values obtained in steps 1 to 8
10. Click "Create Linode" to deploy the system

---------------------------------------------------------------

##### A bit more detailed

**NOTE:** These quick start demos are only intended for use on the Linode platform using the supplied [StackScript](https://cloud.linode.com/stackscripts/635271) to demonstrate example usage cases for the Agile Deployment Toolkit as quickly and easily as possible.  The demos themselves are very quickly put together simply there for illustrative purposes they are not there to provide any truly useful function. If anyone would like to spend time crafting demos useful for real function that could be listed here that would be valued. 

The purpose of these quick start demos is to show you that with just some parameters you can achieve a lot using the Agile Deployment Toolkit with the possibility of going much deeper into it if you choose to.

For more information about parameter configuration please see the [spec](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/templatedconfigurations/specification.md) and [quickspec](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/templatedconfigurations/quick_specification.dat)

**These initial steps are only required the first time you make a deployment. The result of these first steps can be used repeatedly for subsequent demo deployments.** 

--------------------------
<span style="color:red">**YOUR ONE TIME INITIAL PREPARATORY STEPS**</span>

--------------------------

You need the the following data items available to run the quick start demos. The advice is to collate the necessaries and store them in a file **~/adt-credentials.txt** for use once you are ready to deploy one of the demo scenarios. 

1. The public key from an SSH Key Pair from your the laptop you are using the ADT from
3. The public IP address of your laptop. 
4. The Usename of your linode.com account and the email address of your linode.com account
5. A temporary (delete it after you have finished running the demos, Personal Access Token with full access rights for Linode
6. Object Storage Access keys (access and secret) with full access rights to the gb-lon region
7. A domain name with a registrar of your choice using the linode nameservers (www.nuocial.uk) for example. You can use [primary or secondary domains](https://techdocs.akamai.com/cloud-computing/docs/create-a-domain)
8. A VPC in the gb-lon region called adt-vpc

Once you have collated all this information your **~/adt-credentials.txt** the complete file you will need to perform a build should look similar to:

````LAPTOP PUBLIC KEY: "ssh-ed25519 AAAAC3NzaC1lZD.......zGBarK5v+b6NNg4Yxqk16iJ1qsYb8N adt-laptop"````  

````LINUX USERNAME: "agile-deployer"  (my free choice this will be the username that you ssh onto your build machine with)````  

````LINUX USER PASSWORD: "XXXXXXXXXXXX" (my free choice make sure its strong)```` 

````SSH PORT: "1035" (my free choice but for the sake of the demos its recommended to set it to 1035)````  

````PUBLIC IP ADDRESS OF LAPTOP: "143:34:41.21" (the ip address of my laptop)````

````S3 ACCESS KEY:  "XHGBD8edjDBJjBDU2" (generated from linode gui for the gb-lon region with full access)````  

````S3 SECRET KEY:  "jkdjnvunrvvoiinrvovinoi30ejfoinrviunjqkljwnfef" (for the gb-lon region with full access)````  

````LINODE PAT : "difhinrgfgi84f9j4f9j302rrijfubnervggihjjregi34nfin4fine4fi2" (full access rights)````  

````LINODE USERNAME (Cloudhost account ID) : "mylinodeusername" (the username for my linode account)````  

````DNS USERNAME (Cloudhost account email address) : "linodeuseremail@gmail.com" (the email address of my linode account)````  

````DNS SECURITY KEY (same as the full access PAT above) : "difhinrgfgi84f9j4f9j302rrijfubnervggihjjregi34nfin4fine4fi2"````  

````WEBSITE NAME : "nuocial" (if website url = www.nuocial.uk this is "nuocial" if its www.tester.co.uk this is "tester")````  

````WEBSITE URL (the full domain name you have registered with your DNS registrar, no https needed here) : "adtdemo.nuocial.uk"````  

----------------------------

### DNS

To use Linode's DNS Manager as the authoritative name servers for your domain, you need to change the name servers on your registrar to:  

NS1.LINODE.COM
NS2.LINODE.COM
NS3.LINODE.COM
NS4.LINODE.COM
NS5.LINODE.COM

And then add your domain to Linode DNS Manager as a primary domain

![](images/adt-demos-dns.png "Adt-demo DNS 1") 

### VPC

Now you need to setup your VPC. I'll just show you the images of mine
MAKE SURE YOUR SUBNET RANGE IS **10.0.1.0/24** and THE VPC NAME is **adt-vpc** and is in the **gb-lon** region  

The whole (adt-vpc) setup

![](images/adt-demo5.png "Adt-demo 5") 

And the subnet:  

![](images/adt-demo6.png "Adt-demo 6") 


### FIREWALL

And now the firewall:  


![](images/adt-demos-firewall1.png "Adt-demo firewall 1") 

What it looks like (remember to hit the save button)  

![](images/adt-demos-firewall2.png "Adt-demo firewall 2") 


So using your values from your **~/adt-credentials.dat** file you can populate your Stackscript which will then look similar to:

![](images/stackscript1.png "Stackscript 1")  
![](images/stackscript2.png "Stackscript 2")  

And now setup the rest of your linode and create it

![](images/adt-demo1.png "Adt-demo 1")  
![](images/adt-demo2.png "Adt-demo 2") 
![](images/adt-demo3.png "Adt-demo 3") 
![](images/adt-demo4.png "Adt-demo 4") 

The build will then run and you can check the ssh onto the machine using the username you gave and password to do a sudo and check the logs to see how the build is progressing and if there are any problems. 



