There are two different techniques that I use to synchronise filesystems. One is the lightweight method and the other is the heavyweight method. 

#### Lightweight

There are limitations to what the light weight method can do. It's based on filesystem events using inotifywait. The lightweight method is fine if you are adding, deleting and modifying individual files from the filesystem you are monitoring but if you dump a whole directory of files into the directory you are monitoring then the "burst" of events to inotifywait means that some events get lost and therefore not processed by inotifywait. So, the lightweight method can be used to synchronise configuration directories between machines (which will most likely only involve the creation, modification and deletion) of individual files at a time.

The basic operation of the lightweight method is

Set a directory to synchronise between machines
When there is a modification, creation or deletion event in the directory you are monitoring process the event

creation and modification events: Put the created/modified file to the datastore 
deletion: mark a file for deletion in the datastore

As part of the process, created and modified files from other machines written to the datastore will be synced to the local machine meaning that updates to one directory are distributed to all the machines

As another part of the process, any file that one machine has marked for deletion is deleted from the local machine meaning that a delete on one machine is reflected on the other machines in the active set of machines. 

So, to reiterate the lightweight method is definitely not suitable for synchronising webroot directories where an application developer might install an extension which writes tens of files to one of the webroots, some of which will be lost for sure because of the inotifywait limitation.

If you have the skills, this is very modular you can plug and play a different process than what I have developed for the lightweight method simply by refactoring the script: 

>     ${HOME}/services/datastore/filesystems-sync/lightweight/FileSystemSyncController.sh

on your machines.

Please note the lightweight way of synchronising directories needs a cron task added to all the machines that you want to be in the synchronising federation such as:

>     @reboot export HOME="${HOME}" && ${HOME}/services/datastore/config/ActivateConfigDatastoreLightweight.sh"

#### Heavyweight

The heavyweight method is recommended if you want to sync webroots between machines. Again this is very modular so if you want to plug in your own implementation you can. 

To synchronise webroots between your webservrs you need to have a cron entry/entries on each webserver such as:

>     export HOME="${HOME}" && /bin/sleep 2 && ${HOME}/services/datastore/filesystems-sync/heavyweight/FileSystemsSyncingController.sh '2' '/var/www/html' 'webroot-sync'

The workflow for a heavyweight style of file system synchronisation is as follows:

1. If a machine in the synchronisation federation is considered to have been offline for a length  of time, synchronise make a note that we need to synchronise it with any historical archives to make sure that any updates made to filesystems whilst the current machine was offline are correctly applied.

2. If it is considered that we need historical updates to be processed then:

>    ${HOME}/services/datastore/filesystems-sync/heavyweight/ProcessIncomingHistoricalFilesystemUpdates.sh

If historical updates do not need to be processed call:

>     ${HOME}/services/datastore/filesystems-sync/heavyweight/ProcessOutgoingFilesystemUpdates.sh "${target_directory}" "${bucket_type}"
>     ${HOME}/services/datastore/filesystems-sync/heavyweight/ProcessIncomingFilesystemUpdates.sh "${target_directory}" "${bucket_type}"

3. In all cases process additions and deletions as recorded in the datastore:

>     ${HOME}/services/datastore/filesystems-sync/heavyweight/HousekeepAdditionsSyncing.sh "${target_directory}" "${bucket_type}"
>     ${HOME}/services/datastore/filesystems-sync/heavyweight/HousekeepDeletionsSyncing.sh "${target_directory}" "${bucket_type}"


And so the sum total of the core process of heavyweight file system synchronisation is:

on each machine in the federation to be synchronised:

Process any historical updates if needs be
Collate and process all outgoing updates from the filesystem local to the current machine
Collate and process all incoming updates from the datastore and apply them to the local filesystem
Perform housekeeping around additions and deletions (in other words, consider the additions and deletions that have been processed to be historical now)


