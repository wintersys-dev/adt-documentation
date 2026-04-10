OVERRIDING HTACCESS FOR APACHE

Some applications are supplied with a recommended .htaccess file for use when you are deploying for apache and by default I activate the supplied file. However, you may want to override the default file (for example, I had a problem with the default htaccess file for drupal which I needed to make a modification to to resolve) and so I override the default htaccess file for drupal with my customised htaccess file. 

Its simple to override the default htaccess file. all you have to do is have your modified htaccess file available within the sourcecode for your webserver. So, in my mentioned drupal example I create a file called "htaccess.conf" in the correct place and the contents of this file will then override the default htaccess file supplied with drupal. If I don't provide this file then the supplied default htaccess file is used. 

So, in the sourcecode repository for my webserver machine to override the default htaccess file for a drupal deployment, I create a file at:


>     ${HOME}/services/webserver/configuration/drupal/apache/online/repo/htaccess.conf

if I am making a repo style build and 

>     ${HOME}system/webserver/configuration/drupal/apache/online/source/htaccess.conf

if I am naking a source style build.

And so if you create a htaccess.conf file in the correct place that will be used as the htaccess file for your current deployment to the apache webserver. 
