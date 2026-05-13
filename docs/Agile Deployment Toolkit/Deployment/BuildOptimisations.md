#### BUILD OPTIMSATION RECOMMENDATIONS

1. Set up a reusable library of cloud-init or user-data scripts that can be reused with minor modifications for multiple builds
2. Set up a permanent build machine that can be used for all your builds. This will be faster once set up because the build machine doesn't have to be built and configured for each build cycle like it does with the hardcore build style so that's one less machine in the build cycle
3. You can set IN_PARALLEL to "1" in your template which means your servers (webservers, database, autoscalers) and so on will be provisioned in parallel rather than sequentially which can hasten your build process substantially.
4. You can generate snapshots to build your machines from which can be a speed improvement (as well as less resource intensive) because the snasphot already has the software that your machine needs installed on it. Please see [generate snapshots](https://www.wintersys-dev.uk/Agile%20Deployment%20Toolkit/Deployment/BuildFromSnapshots/)



