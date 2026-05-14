#### GIT PERSONAL ACCESS TOKENS

The prefered way to access git services such as Github, Bitbucket and Gitlab repositories is to use a personal access token instead of a password. 
You can generate personal access tokens through their respective GUI systems, for example, [PAT](https://www.github.com/settings/tokens).
You can then use this token in place of a password when using these providers by setting the environment variable:  

**APPLICATION_REPOSITORY_TOKEN** 

**MAKE SURE THE TOKEN IS GIVEN THE RIGHTS TO ADMINISTER AND DELETE REPOSITORIES, OTHERWISE THERE WILL BE FAILURES RELATING TO BACKUPS AND SO ON**   

If your token has a limited expiration time you will need to keep an eye on that because if your deployment is deployed for longer than the expiration time of your token then services that rely on your git provider will start to fail (you should receive warning emails of course) if the token expires. To be safe you can set the token to never expire but most likely the Git provider you use advises you not to do that. 
