#### THE HEAVYWEIGHT WEBSERVER SYNCING PROCESS

A common issue with multi webserver configurations is keeping the webroot directories in sync. Often this is achieved by mounting a single filesystem over NFS or some other solution meaning that there is a single point of truth but unless you have a serious setup doing it that way is a single point of failure. It can be done using elastic file systems in the same way but in my case not all the cloudhost providers I support have an option for elastic filesystem usage so that isn't a runner for all my use cases. So, (let me know if there's some other easy win way that I am overlooking) but, I wrote a bespoke script to perform the synchronisation between the webroots of the n webservers that are currently deployed in my setup. 

I will give you a brief overview of how the webroot synchronisation method that I use works. I have tested it as best I can but its actually quite a tricky issue to get right in a satisfactory way. 

NOTE: You will have to synchronise your webroots if you have multiple webservers as it is the webroot synchronisation process that pushes out updates that you make as an administator to, for example, configuration.php for joomla or wp-config.php for wordpress to all the servers in your server fleet. 

So, the logic of the webroot synvhronisation process from a high level is as follows:

1. Each webserver has a set of incoming and outgoing file system changes and these changes are categorised as additions and deletions
2. Each webserver writes its current additions and deletions to the datastore in a machine identifiable way in order for the other webservers to be able to apply the updates to themselves
3. The webservers only update their additions if the updated files are newer than the current file for unlikely event where the same file gets updated on one webserver and then another before the new addition has been applied
4. additions and deletions are kept historically for an arbitrary period of time. In other words, the system can apply the previous n minutes of additions and deletions at any time it chooses and what this gives is an n minute grace period if a server were to be rebooted, for example, as whilst it is rebooting, without historical records, additions and deletions may well be lost

As a high level overview that is what is happening with the heavyweight (recommended) webroot syncing process

#### THE LIGHTWEIGHT WEBSERVER SYNCING PROCESS
