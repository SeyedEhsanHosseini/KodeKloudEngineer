## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Check current iptables rules 

```
iptables -L
```
## Add requested rules to iptables

```
iptables -A INPUT -p tcp --dport <port> -j ACCEPT
iptables -A INPUT -p tcp --dport <port> -j REJECT

```
## Save new rules permanently 

```
service iptables save
```
```
iptables-save
```


