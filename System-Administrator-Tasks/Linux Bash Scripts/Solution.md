## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## create a bash script and add below lines to it, then save and exit

```
vi /scripts/ecommerce_backup.sh
```

```
#!/usr/bin/bash

zip -r /backup/xfusioncorp_ecommerce.zip /var/www/html/ecommerce

scp /backup/xfusioncorp_ecommerce.zip clint@stbkp01:/backup/

```


#### make the script executable using chmod

```
chmod +x /scripts/ecommerce_backup.sh

```

## create an ssh-key pair and copy public key on backup server for using scp command without asking password


```
ssh-keygen
```

```
ssh-copy-id -i /home/tony/.ssh/id_rsa.pub clint@stbkp01
```

#### Output

```
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'clint@stbkp01'"
and check to make sure that only the key(s) you wanted were added.

```

