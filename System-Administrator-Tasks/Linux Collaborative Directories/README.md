## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Create requested directories recursively

```
mkdir -p /path/to/folder
```
## Check current permissions of the path using ls command

```
ll -lsd /path/to/folder
```

## Change primary group for the directories recursively

```
chgrp -R <groupName> /path/to/folder

```

## Modify permissions for the directories recursively

```
chmod -R 2770 /path/to/folder

```

## To verify changes

```
ll -lsd /path/to/folder
```


