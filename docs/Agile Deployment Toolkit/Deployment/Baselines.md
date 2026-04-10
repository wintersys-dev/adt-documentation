If you have got a website running then you might want to make a baseline of it. This baseline can either be public and deployed by others (taking all necessary precautions to make sure that you don't have any sensitive credentials in your public repositories) or they can just be personal meaning that you have a baseline to work off down the road.

A baseline can be developed under one domain name and deployed to another domain name without problem and that's what makes a baseline quite a powerful concept. 

A baseline will consist of a copy of your webroot in a git repository and a dump of your database in a separate repository. These two repos are then consulted by a fresh build in order to reconstitute the baselined application as a brand new application.

To create a baseline you can do it from your build machine by running

>     ${BUILD_HOME}/helpers/PeformWebsiteBaseline.sh  


and

>     ${BUILD_HOME}/helpers/PerformDatabaseBaseline.sh  


The scripts should be straight forward to follow as they run. Rudimentary checking is done that your baseline is created correctly but you should always go take a look to insure that the files you expect to be there once the scripts are run are written to the repository. 

