#### HOW TO GENERATE BESPOKE BASELINES OF YOUR WEBROOT AND DATABASE

If you have deployed a virgin application and customised it and got a bespoke website running then you might (or probably will) want to make a baseline of it. This baseline can either be public and deployed by others (taking all necessary precautions to make sure that you don't have any sensitive credentials in your public repositories) or they can just be personal meaning that you have a baseline stored in a private repository to work off and use in private because you will need your APPLICATION_REPOSITORY access keys and so on to be able to deploy it. A baseline can be developed under one domain name and deployed to another domain name without problem and that's what makes a baseline quite a powerful concept. I can therefore develop a baseline against my own domain (dev.wintersys.uk) and you can take my baselined database and code repositories and deploy them to your custom domain (prod.customer.uk) and as far as I know there shouldn't be any issues because the domain names are swapped out with placeholder tokens when the baseline is being generated and those placeholder tokens are swapped out for a live customer domain when the baseline is being deployed. If there are any unforeseen issues, please let me know. 

A baseline will consist of a copy of your webroot in a git repository and a dump of your database in a git repository also. Git repositories have hard limits on what size they can be so you can't make a baseline of a database that is GB and GB in size because the git repository won't accept it but the baselines are not really meant to be for huge amounts of data the baselines are supposed to be "empty" applications meaning not much user data and then the production mode deployments are intended to be filled with user data and can be much larger in size because they are stored in S3 not in git repositories.  These two repos (the webroot and database) are then consulted by a fresh build in order to reconstitute the baselined application as a brand new deployed application.

To create a baseline of a bespoke application you have developed you can do it from your build machine by running

>     ${BUILD_HOME}/helpers/baseline/PeformWebsiteBaseline.sh  


and

>     ${BUILD_HOME}/helpers/baseline/PerformDatabaseBaseline.sh  


The scripts should be straight forward to follow as they run. Rudimentary checking is done that your baseline is created correctly but you should always go take a look to insure that the files you expect to be in the repository are there once the scripts are completed.  

