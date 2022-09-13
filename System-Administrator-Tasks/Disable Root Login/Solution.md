## Login to the respective mentioned server in the task using ssh
```
ssh username@server
```

## Edit the SSH config file by using the command below:
```
sudo vi /etc/ssh/sshd_config
```

 
## Find the line that says PermitRootLogin:

```
#PermitRootLogin yes
```

## Now you can uncomment that line and edit it to be like this:

```
PermitRootLogin no
```

## Save the config file and Exit then reload the sshd service using systemctl command:

```
sudo systemctl reload sshd.service
```


