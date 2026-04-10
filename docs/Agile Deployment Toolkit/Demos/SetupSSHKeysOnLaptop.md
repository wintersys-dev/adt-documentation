#### Obtain SSH Keypair for your laptop machine for usage in ADT Quick Demos 

1. Check for existing SSH Keypairs if you are not sure.

>     /bin/ls -l ~/.ssh/id*

If you see a message "No such file or directory" that means that you don't have any existing SSH keypairs.

----------------------

2. Depending on whether you have existing keypairs or not you can decide to use one of your existing keys or whether to generate a new keypair.
   If you have existing keys and you are generating new a new keypair be careful not to overwrite your existing keys. If you are using an existing
   keypair that you already had, skip to step 4

-----------------------

3. Geneate a new keypair. You can choose your prefered algorithm from **ONE** of the following:

>     1. /usr/bin/ssh-keygen -t rsa -b 4096 -C "adt-laptop"
>     2. /usr/bin/ssh-keygen -t ecdsa -b 521 -C "adt-laptop"
>     3. /usr/bin/ssh-keygen -t ed25519 -b 521 -C "adt-laptop"

You will be now prompted to choose the location for the key pair.
Press Enter to save the key pair in the default location (~/.ssh/id_rsa).
You may be warned if you are about to overwrite any existing SSH keys and if you very sure you are OK with that press Y.
You will also be prompted for a passphrase to access your keys, its advisable to enter one. 

------------------------------

4. Assuming that your key is located in **~/.ssh/id_rsa** or **~/.ssh/id_ecdsa** or **~/.ssh/id_ed25519** depending on which algorithm you chose, type **ONE** of:

>     1. /bin/grep "adt-laptop" ~/.ssh/id_rsa.pub
>     2. /bin/grep "adt-laptop" ~/.ssh/id_ecdsa.pub  
>     3. /bin/grep "adt-laptop" ~/.ssh/id_ed25519.pub

This will give you your public key which will look similar to **ONE** of the following depending on your algorithm:

>     1. ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQD...........KfwdTda8LM8Ll6CNupE6/+lanhURM9HDNX47Q== adt-laptop
>     2. ecdsa-sha2-nistp521 AAAAE2VjZHNhLXN...........dYR+eN/dA4ISk5awQfKzug== adt-laptop
>     3. ssh-ed25519 AAAAC3NzaC1lZDI1NTE.........arK5v+b6NNg4Yxqk16iJ1qsYb8N adt-laptop

-----------------------------

5. Whatever output you have from step 5 is your public key. You need to have a copy of your public key to use later on, add it to your adt-credentials.txt file by chooding the appropiate **ONE** of the following depending on your algorithm:

>     1. /bin/echo "MY ADT PUBLIC KEY: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQD...........KfwdTda8LM8Ll6CNupE6/+lanhURM9HDNX47Q== adt-laptop" >> ~/adt-credentials.txt
>     2. /bin/echo "MY ADT PUBLIC KEY: ecdsa-sha2-nistp521 AAAAE2VjZHNhLXN...........dYR+eN/dA4ISk5awQfKzug== adt-laptop" >> ~/adt-credentials.txt
>     3. /bin/echo "MY ADT PUBLIC KEY: ssh-ed25519 AAAAC3NzaC1lZDI1NTE.........arK5v+b6NNg4Yxqk16iJ1qsYb8N adt-laptop" >> ~/adt-credentials.txt








