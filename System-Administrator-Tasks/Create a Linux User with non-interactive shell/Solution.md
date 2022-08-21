## Login to the respective mentioned server in the task using ssh

```
ssh username@server 
```
## Check user is existed on the server or not

```
id <username>
```

## If the user is not found the then you can create a user with a non-interactive shell 

```
sudo adduser <username> -s /sbin/nologin
```

## Validate user is created successfully 

```
id <username> 

cat /etc/passwd | grep <username>
```
