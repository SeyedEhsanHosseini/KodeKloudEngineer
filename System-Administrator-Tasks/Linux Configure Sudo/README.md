## Login to the respective mentioned server in the task using ssh then switch to root user
```
ssh username@server

sudo su -
```

## Add user to sudoers group:
```
usermod -aG wheel <username>
```

 
## To Edit the Sudoers File:

```
visudo
```

## Under "Same thing without a password" line:

```
<username>          ALL=(ALL)       NOPASSWD: ALL
```


