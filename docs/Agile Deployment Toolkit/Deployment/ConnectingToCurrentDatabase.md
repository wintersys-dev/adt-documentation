#### UTILITY SCRIPTS FOR CONNECTING TO DATABASES

If you are on a webserver and your application is using an SQL based database you can connect straight to your tables by running:

>     run ${HOME}/utilities/remote/ConnectToRemoteMYSQLDB.sh

If you are on a webserver and your application is using an Postgres based database you can connect straight to your tables by running:

>     run ${HOME}/utilities/remote/ConnectToRemotePostgresDB.sh

If you are on your database machine already and running an SQL database rarther than faffing around with credentials and so on you can run:

>     run ${HOME}/utilities/remote/ConnectToMySQLDB.sh

If you are on your database machine already and running a Postgres database rather than faffing around with credentials and so on you can run:

>     run  ${HOME}/utilities/remote/ConnectToPostgresDB.sh

These scripts will connect to whatever your current database configuration is regardless of whether the database is self managed and local or managed as a service for you by a provider. 
