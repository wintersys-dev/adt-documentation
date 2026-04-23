The heart of this toolkit is the templating system. You set the configurations you want in your templates and the build process will be dependant on the values that you set.  

What I want to do here is simply show you how you might go about configuring your template values for some different scenarious you might like to configure your deployment to support. When making a deployment, you should refer to the [spec](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/specifications/specification.md) and the [quickspec](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/specifications/quick_specification.dat)

Any valid configuration will undoubtably have some combination of these scenarios for the deployment to be successful, for example, its no use configuring this toolkit make a Postgres database deployment if you are deploying Wordpress because as far as I know Wordpress doesn't support postgres out of the box and so such a configuration would result in a failed build with the way that I do things. Its not impossible for wordpress using postrgres to be supported here, but, I chose not to because Postgres is not commonly used for wordpress. 

If you look  

[here](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/processing/PreProcessingMessages.sh)  

then about line 170 you will see that this suggested scenario of misconfiguration is checked for but its not clear that all such misconfigurations can be checked for and  therefore, the onus is on you, as a deployer to know what configurations are appropriate for what you are trying to achieve.  

If you are deploying a virgin application you should make modifications to template 1 for your current cloudhost provier. If you are deploying a baselined application you should modify template 2 for your current cloudhost provider and if you are deploying from a temporal backup you should modify template 3 for the appropriate cloudhost.

---------------------------------

#### Objective 1

To deploy a virgin Joomla application you need to set the following values in template 1:

>     export APPLICATION="joomla"
>     export BUILD_ARCHIVE_CHOICE="virgin"

#### Objective 2

To deploy a virgin Wordpress application you need to set the following values in template 1:

>     export APPLICATION="wordpress"
>     export BUILD_ARCHIVE_CHOICE="virgin"


#### Objective 3

To deploy a virgin Drupal application you need to set the following values in template 1:

>     export APPLICATION="drupal" 
>     export BUILD_ARCHIVE_CHOICE="virgin"

#### Objective 4

To deploy a virgin Moodle application you need to set the following values in template 1:

>     export APPLICATION="moodle"
>     export BUILD_ARCHIVE_CHOICE="virgin"


--------------------------------------------------------------

#### Objective 5

To deploy a baselined application in template 2, you modify as in the following example for a joomla application

>     export APPLICATION="joomla"
>     export APPLICATION_BASELINE_SOURCECODE_REPOSITORY="examplewebsite-webroot-sourcecode-baseline"
>     export BASELINE_DB_REPOSITORY="examplewebsite-db-baseline"
>     export BUILD_ARCHIVE_CHOICE="baseline"

#### Objective 6

To deploy a temporal application with hourly peridicity in template 3, you modify as in the following example for a joomla application

>     export APPLICATION="joomla"
>     export BUILD_ARCHIVE_CHOICE="hourly"

----------------------------------------------------------------------

#### Objective 7

To deploy PHP version 8.3 you set the following values in the appropriate template

>     export APPLICATION_LANGUAGE="PHP"
>     export PHP_VERSION="8.3"

#### Objective 8

To deploy PHP version 8.4 you set the following values in the appropriate template

>     export APPLICATION_LANGUAGE="PHP"
>     export PHP_VERSION="8.4"

-----------------------------------------------------------------

#### Objective 9

To enable the datastore you can set the following values appropriately in any of the templates - you can refer to the specification [here](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/templatedconfigurations/specification.md) to see what the HOST_BASE value should be for your datastore and you need to generate the access_key and secret_key for yourself

>     export S3_ACCESS_KEY="xxxxx"
>     export S3_SECRET_KEY="yyyyy"
>     export S3_HOST_BASE="ams3.digitaloceanspaces.com"
>     export S3_LOCATION="US"

--------------------------------------------------------------

#### Objective 10

To mount an application's assets directory set the following values appropriately in template 3. In this example I use joomla where the assets directory is the images subdirectory

>     export DIRECTORIES_TO_MOUNT="images"
>     export PERSIST_ASSETS_TO_DATASTORE="1"

--------------------------------------------------------------

#### Objective 11

To deploy to Debian 12 machines, set these values in any of your templates

>     export BUILDOS="debian"
>     export BUILDOS_VERSION="12"

#### Objective 12

To deploy to Ubuntu 24.04 machines, set these values in any of your templates

>     export BUILDOS="ubuntu"
>     export BUILDOS_VERSION="24.04"

--------------------------------------------------------------

#### Objective 13

To set your DNS provider, set the following values in your templates

>     export DNS_USERNAME="dns_provider_email/Username"
>     export DNS_SECURITY_KEY="xxxx"
>     export DNS_CHOICE="cloudflare"

-----------------------------------------------------------------

#### Objective 14

To set your website characteristics set the following values in any template:

>     export WEBSITE_DISPLAY_NAME="My Example Website"
>     export WEBSITE_NAME="testwebsite"
>     export WEBSITE_URL="www.testwebsite.uk"

-------------------------------------------------------------------

#### Objective 15

Set the repositories which hold your infrastructure sourcecode (the adt that holds this file) in any template, for example:

>     export INFRASTRUCTURE_REPOSITORY_PROVIDER="github"
>     export INFRASTRUCTURE_REPOSITORY_OWNER="wintersys-dev"
>     export INFRASTRUCTURE_REPOSITORY_USERNAME="wintersys-dev"


#### Objective 16

Set the repositories which hold your application sourcecode in any template, for example:

>     export APPLICATION_REPOSITORY_PROVIDER="github"
>     export APPLICATION_REPOSITORY_OWNER="adt-demos"
>     export APPLICATION_REPOSITORY_USERNAME="adt-demos"
>     export APPLICATION_REPOSITORY_TOKEN="xxxxx"

------------------------------------------------------------------------

#### Objective 17

Set the SMTP email settings using the following settings in any template:

>     export SYSTEM_EMAIL_PROVIDER="1"
>     export SYSTEM_TOEMAIL_ADDRESS="testemail@testemail.com"
>     export SYSTEM_FROMEMAIL_ADDRESS="testemail@testemail.com"
>     export SYSTEM_EMAIL_USERNAME="xxxx"
>     export SYSTEM_EMAIL_PASSWORD="yyyy"
>     export EMAIL_NOTIFICATION_LEVEL="ERROR"

------------------------------------------------------------------------

#### Objective 18

Set the system to use MariaDB "locally" in any template

>     export DB_PORT="2035"
>     export DATABASE_INSTALLATION_TYPE="Maria"


#### Objective 19

Set the system to use MySQL "locally" in any template

>     export DB_PORT="2035"
>     export DATABASE_INSTALLATION_TYPE="MySQL"


#### Objective 20

Set the system to use Postgres "locally" in any template

>     export DB_PORT="2035"
>     export DATABASE_INSTALLATION_TYPE="Postgres"

------------------------------------------------------------------------

#### Objective 21

Set the system to use the MariaDB as "DBaaS" that you have deployed from the GUI in template 3 for your currrent provider

>     export DB_PORT="2035"
>     export DATABASE_INSTALLATION_TYPE="DBaaS"
>     export DATABASE_DBaaS_INSTALLATION_TYPE="Maria"


#### Objective 22

Set the system to use the MariaDB as an automatically deployed "DBaaS" in template 3 for your currrent provider

>     export DB_PORT="2035"
>     export DATABASE_INSTALLATION_TYPE="DBaaS"
>     export DATABASE_DBaaS_INSTALLATION_TYPE="xxxx" (please see [here](https://github.com/wintersys-dev/adt-build-machine-scripts/blob/main/doco/AgileToolkitDeployment/DeployingDBaaS.md) for explanation) 

-------------------------------------------------------------------------

#### Objective 23

To Set the deployment details for your server machines use the following settings in any template (here I use digital ocean as an example):

>     export REGION="lon1"
>     export DB_SIZE="s-1vcpu-1gb"
>     export DB_SERVER_TYPE="s-1vcpu-1gb"
>     export WS_SIZE="s-1vcpu-1gb"
>     export WS_SERVER_TYPE="s-1vcpu-1gb"
>     export AS_SIZE="s-1vcpu-1gb"
>     export AS_SERVER_TYPE="s-1vcpu-1gb"
>     export CLOUDHOST="digitalocean"
>     export MACHINE_TYPE="DROPLET"

----------------------------------------------------------------------------

#### Objective 24

Set the default number of webservers and autoscalers by manipulating the following settings in your templates, for example:

>     export NO_AUTOSCALERS="0"
>     export NUMBER_WS="1"

------------------------------------------------------------------------------

#### Objective 25

Set the webserver type in any of your templates to APACHE

>     export WEBSERVER_CHOICE="APACHE"

#### Objective 26

Set the webserver type in any of your templates to NGINX

>     export WEBSERVER_CHOICE="NGINX"

#### Objective 27

Set the webserver type in any of your templates to LIGHTTPD

>     export WEBSERVER_CHOICE="LIGHTTPD"

----------------------------------------------------------------------------

#### Objective 28

Set the timezone for your servers in any of your templates:

>     export SERVER_TIMEZONE_CONTINENT="Europe"
>     export SERVER_TIMEZONE_CITY="London"

-------------------------------------------------------------------------------

#### Objective 29

To switch to development mode:

>     export PRODUCTION="0"
>     export DEVELOPMENT="1"

--------------------------------------------------------------------------------

#### Objective 30

To switch to production mode:

>     export PRODUCTION="1"
>     export DEVELOPMENT="0"

------------------------------------------------------------------------------

#### Objective 31

To modify which firewalls are active for example for native and ufw to be enabled in any template set like this:

>     export ACTIVE_FIREWALLS="3"

------------------------------------------------------------------------------

#### Objective 32

To switch "SSL_LIVE_CERT" on and off:

>     export SSL_LIVE_CERTN="1" (this will have issuance limits/week but will be a production ready certificate)

and

>     export SSL_LIVE_CERT="0" (this will not have issuance limits but will only be a staging certificate)

------------------------------------------------------------------------------

#### Objective 33

To make the machines build in parrallel rather than sequentially, put:

>     export IN_PARALLEL="1"

---------------------------------------------------------------------------------




















