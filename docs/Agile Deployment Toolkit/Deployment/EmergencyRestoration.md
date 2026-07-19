EMERGENCY RESTORATION

If you are running a CMS system and there is a bad update applied (extension or plugin for example) then that can take your site offline. The emergency restoration feature is here so that you can restore to a previously working timestamped backup in short order to get your site back online.
To perform an emergency restoration is straight forward.

>     ssh onto your build machine
>     cd ${BUILD_HOME}/helperscripts/backup
>     /bin/sh ./EmergecyResstoration.sh

You will then be shown a list of time stamped archives that are available in your datastore and you need to enter the name of the archive exactly as it appears in the list, for example:

>     ARCHIVE.2026-07-07-16hr

This can help you get out of a sticky spot if you happen to suffer either a hack or a corrupted update or some other problem that takes your site offline. To be able to quickly restore to a previous version that is known to be right functioning in less than 5 minutes could help you avoid quite a bit of stress if the worst were to happen.
