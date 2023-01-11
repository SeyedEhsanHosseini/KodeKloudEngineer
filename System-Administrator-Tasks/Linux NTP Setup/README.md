## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Install NTP Server using yum package manager 

```
yum install ntp -y
```
## Configure NTP Client to Synchronize with NTP Server

#### To synchronize the time of your local Linux client machine with NTP server, edit the /etc/ntp.conf file on the client side.

```
vi /etc/ntp.conf
```

