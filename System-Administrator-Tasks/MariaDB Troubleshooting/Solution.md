## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## Check the current status of mariadb service using systemctl

```
systemctl status mariadb.service
```

#### You will get below output

```
● mariadb.service - MariaDB database server

   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; disabled; vendor preset: disabled)

   Active: inactive (dead) 
```

## Try to start mariadb using systemctl


```
systemctl start mariadb.service
```

#### You will get below output

```
Job for mariadb.service failed because the control process exited with error code.
See "systemctl status mariadb.service" and "journalctl -xe" for details.
```

## To see more detail on previous error use -l flag

```
systemctl status mariadb.service -l
```

#### You will get below output

```
...

stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[734]: 
Database MariaDB is not initialized, but the directory /var/lib/mysql is not empty, so initialization cannot be done.

...

```

## Check the following path o see ownerships, groups and directory names

```
ls -la /var/run/

```


#### You will get below output

```
...

drwxr-xr-x 2 root mysql 4096 <MONTH> <DAY> <YEAR>     mariadb

...

```

## Change ownership of mariadb directory recursively using chown command

```
chown -R mysql:mysql /var/run/mariadb/
```

## Start and enable mariadb service then check it's status using systemctl

```
systemctl start mariadb.service

systemctl enable mariadb.service

systemctl status mariadb.service

```

