## Login to the respective mentioned server in the task using ssh

```
ssh username@server
```
## Open /etc/resolv.conf file and paste following lines in it then save & exit

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

## To verify the change 

```
ping any domain/hostname. If you get “Failed to resolve host:” error, then there is something wrong in network configuration (apart from DNS).

```
