## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Install IPtables services  

```
yum install -y iptables-services
```

## Start & enable the IPtables service  

```
systemctl start iptables && systemctl enable iptables

```

## Add below IPtables Rules 

###### replace \<port\> with Apache's port mentioned in the task

###### replace \<ip-address\> with Nautilus HTTP LBR IP address from wiki page (Infrastructure Details)

```
iptables -A INPUT -p tcp --destination-port <port> -s <ip-address> -j ACCEPT

iptables -A INPUT -p tcp --destination-port <port> -j DROP

```
#### Get the iptables list with the line numbers enabled

```
iptables -L --line-numbers
```

#### Add below IPtables Rule

```
iptables -R INPUT 5 -p icmp -j REJECT
```

## Save the rules to ensure they remain persistent across reboot

```
service iptables save
```
