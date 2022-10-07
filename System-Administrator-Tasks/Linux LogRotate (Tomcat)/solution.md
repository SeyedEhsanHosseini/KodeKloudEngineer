## Login to the respective mentioned server in the task using ssh then switch to root user
```
ssh username@server

sudo su -
```
## Install requested package using yum package manager 

```
yum install -y tomcat
```
## Start and enable the package

```
systemctl start tomcat.service

systemctl enable tomcat.service

```
## Edit logrotate configuration file for tomcat, Save and Exit

```
vi /etc/logrotate.d/tomcat 
```
## Apply new configuration 

```
logrotate /etc/logrotate.d/tomcat
```
