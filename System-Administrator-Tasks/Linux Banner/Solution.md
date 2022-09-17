## Copy banner file from local server to remote server using scp

```
scp /home/thor/nautilus_banner remoteuser@remoteserver:/home/<username>/nautilus_banner
```

## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## Check the sshd configuration file from below path and looking for a line that starts with "banner"

```
cat /etc/ssh/sshd_config

```


#### You will get below output

```
...

#banner none

...

```

## Edit the postfix main configuration file from below path and uncomment "banner" line


```
vi /etc/ssh/sshd_config
```

```
banner /home/<username>/nautilus_banner
```

## Restart sshd service using systemctl

```
systemctl restart sshd.service

```

