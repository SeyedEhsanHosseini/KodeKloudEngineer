## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## Install PWAUTH using yum with --enablerepo=epel 
```
yum --enablerepo=epel -y install mod_authnz_external pwauth
```

## Edit "authnz_external.conf" file and Add below lines to it then save and exit
```
vi /etc/httpd/conf.d/authnz_external.conf
```
#### Add:
```
<Directory /var/www/html/protected>

    AuthType Basic

    AuthName "PAM Authentication"

    AuthBasicProvider external

    AuthExternal pwauth

    require valid-user

</Directory>
```
#### Create a protected directory on this path
```
mkdir -p /var/www/html/protected
```

## Start the service then check its status 
```
systemctl start httpd && systemctl status httpd
```
#### Output:
```
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Sun 2022-11-06 20:07:02 UTC; 8ms ago
     Docs: man:httpd(8)
           man:apachectl(8)
 Main PID: 960 (httpd)
   Status: "Processing requests..."
   CGroup: /docker/dba8884be5d005dac98685455f01b5c4d50b9fc2964747651846ea36c9cc42c9/system.slice/httpd.service
           ├─960 /usr/sbin/httpd -DFOREGROUND
           ├─962 /usr/sbin/httpd -DFOREGROUND
           ├─963 /usr/sbin/httpd -DFOREGROUND
           ├─965 /usr/sbin/httpd -DFOREGROUND
           ├─966 /usr/sbin/httpd -DFOREGROUND
           └─967 /usr/sbin/httpd -DFOREGROUND

Nov 06 20:07:02 stapp02.stratos.xfusioncorp.com systemd[1]: Starting The Apache HTTP Server...
Nov 06 20:07:02 stapp02.stratos.xfusioncorp.com httpd[960]: AH00558: httpd: Could not reliably determine the server's fully qualified domain name, us...message
Nov 06 20:07:02 stapp02.stratos.xfusioncorp.com systemd[1]: Started The Apache HTTP Server.
Hint: Some lines were ellipsized, use -l to show in full.
```


## Validate the task by running this:
```
curl -u <username>:<password> http://localhost:8080/protected/
```

#### Output must be like this:
```
This is KodeKloud Protected Directory
```




















































