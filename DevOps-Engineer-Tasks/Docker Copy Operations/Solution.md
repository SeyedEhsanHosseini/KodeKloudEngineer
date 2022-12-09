## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## list running containers
```
[root@stapp03 ~]# docker ps
```
#### Output:
```
CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS              PORTS     NAMES
f8884b3c4bec   ubuntu    "bash"    About a minute ago   Up About a minute             ubuntu_latest
```
## Copy a file from docker host to ubuntu_latest container in /tmp/ location

```
[root@stapp03 ~]# docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```
## Validate the task by listing files in /tmp directory

```
[root@stapp03 ~]# docker exec ubuntu_latest ls -l /tmp/
```
#### Output:
```
total 4
-rw-r--r-- 1 root root 74 Dec  9 10:10 nautilus.txt.gpg
```
