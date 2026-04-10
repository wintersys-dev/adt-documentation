
There are several ways that the time to build your servers can be shortened but they all require a little bit of upfront investment. 

1. If you follow the Quick Demos/Hardcore approach then the build will take longer because a build machine has to be provisioned and configured before the process of building webservers autoscalers and databases can even start. Using the quick build approach a new build machine is provisioned and configured for each build cycle. Using this approach might also take longer because you have to get it right because there's no sanity checking (that's why its hardcore) so if you have got a typo in your deployment settings then the build will likely fail and you will have to restart the whole process including time spent figuring out why it failed. Its a good apprach for quick demos and so on where the settings have been sanity checked.

2. If you provision a build machine for the long term that you intend to run multiple build cycles on then the builds will be quicker because the build machine isn't provisioned and configured for each build, it is provisioned once and reused on subsequent builds.

3. You can set your autoscalers, webservers and database machines to build in parallel by setting "INPARALLEL=1" in your template, this will shorten how long a build take by building your servers in parallel. 

4. You can [generate snapshots](https://www.wintersys-dev.uk/Agile%20Deployment%20Toolkit/Deployment/BuildFromSnapshots/) of your machines and make subsequent deployments using the snapshots instead of building from scratch that should give you a speed boost. 
