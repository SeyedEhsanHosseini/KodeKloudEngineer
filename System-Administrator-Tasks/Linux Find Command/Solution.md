## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## You can combine find and cp commands together like this:

```
find /path/to/directory/ -type f -name "*.ext" -exec cp --parents {} /destination/ \;
```

#### More Info

```
find:
     -type f           --> To find files
    
     -name <*.ext>     --> To find files that their names ends with .ext format 
     
     -exec <command>   --> Allows us to execute commands on results
     
cp:
     --parents         --> To copy files preserving directory path     
```

