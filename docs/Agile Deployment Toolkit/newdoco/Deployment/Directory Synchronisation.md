#### THE HEAVYWEIGHT WEBSERVER SYNCING PROCESS

A common issue with multi webserver configurations is keeping the webroot directories in sync. Often this is achieved by mounting a single filesystem over NFS or some other solution meaning that there is a single point of truth but unless you have a serious setup doing it that way is a single point of failure. It can be done using elastic file systems in the same way but in my case not all the cloudhost providers I support have an option for elastic filesystem usage so that isn't a runner for all my use cases. So, (let me know if there's some other easy win way that I am overlooking) but, I wrote a bespoke script to perform the synchronisation between the webroots of the n webservers that are currently deployed in my setup. 

I will give you a brief overview of how the webroot synchronisation method that I use works. I have tested it as best I can but its actually quite a tricky issue to get right in a satisfactory way. 

NOTE: You will have to synchronise your webroots if you have multiple webservers as it is the webroot synchronisation process that pushes out updates that you make as an administator to, for example, configuration.php for joomla or wp-config.php for wordpress to all the servers in your server fleet. 

So, the logic of the webroot synchronisation process from a high level is as follows:

1. Each webserver has a set of incoming and outgoing file system changes and these changes are categorised as additions and deletions
2. Each webserver writes its current additions and deletions to the datastore in a machine identifiable way in order for the other webservers to be able to apply the updates to themselves
3. The webservers only update their additions if the updated files are newer than the current file for unlikely event where the same file gets updated on one webserver and then another before the new addition has been applied
4. additions and deletions are kept historically for an arbitrary period of time. In other words, the system can apply the previous n minutes of additions and deletions at any time it chooses and what this gives is an n minute grace period if a server were to be rebooted, for example, as whilst it is rebooting, without historical records, additions and deletions may well be lost

As a high level overview that is what is happening with the heavyweight (recommended) webroot syncing process

#### THE LIGHTWEIGHT WEBSERVER SYNCING PROCESS

The lightweight technique for synchronising directories ISN'T suitable for all scenarios and that's why it is called "lightweight". The lightweight solution only works for filesystems where there are occassional updates of single files or a handfull of files. It doesn't work if there are mass updates such as might occur if you were performing an application update and so the lightweight method is NOT suitable for webroot synchronisation but it might work well for you for synchronising config directories that only have occassional single config file updates. 

The reason why the lightweight mechanism for directory sync updates doesn't work for bulk updates to the file system is because it uses a tool called "inotifywait" and inotifywait generates events based on interaction with the filesystem. In my testing I found that when many files were updated at once each additional or modified file update creates an event but (in my testing anyway) inotifywait loses or drops events when there is a mass update to the filesystem. 

In terms of the logic of the lightweight directory synchronisation mechanism the processing is as follows:

The inotifywait tool listens for events on the directory that you want to synchronise 

>     /usr/bin/inotifywait -q -m -r -e delete,modify,create ${active_directory} | while read DIRECTORY EVENT FILE 

When an event occurs it is analysed to see if it is a create, modify or a delete event and based on which type of event it is the datastore that is used to synchronise the filesystems is updated to reflect the change that has been initiated. 
