## Login to the respective mentioned server in the task using ssh

```
ssh username@server
```
## You can combine find and cp commands together like this:

```
find /home/usersdata/ -type f -user <username> -exec cp --parents {} /<Destination-Directory>/ \;
```

#### More Info

```
find:
     -type f           --> To find files
    
     -user <username>  --> To find files owned by username
     
     -exec <command>   --> Allows us to execute commands on results
     
cp:
     --parents         --> To copy files preserving directory path     
```

