## Login to the respective mentioned server in the task using ssh then switch to root user
```
ssh username@server

sudo su -
```

## Edit Apache configuration, change port number, then save and exit

```
vi /etc/httpd/conf/httpd.conf
```

#### Output:
```
# Listen: Allows you to bind Apache to specific IP addresses and/or

# Change this to Listen on specific IP addresses as shown below to

#Listen 12.34.56.78:80

Listen <port>

```

##  Create new configuration file in conf.d directory for permanent & temporary redirect configs 

```
vi /etc/httpd/conf.d/main.conf
```

##### write down these line, then save and exit 

```
<VirtualHost *:<port>

ServerName http://stapp03.stratos.xfusioncorp.com:<port>/

Redirect 301 / http://www.stapp03.stratos.xfusioncorp.com:<port>/

</VirtualHost>


<VirtualHost *:<port>

ServerName www.stapp03.stratos.xfusioncorp.com:<port>/blog/

Redirect 302 /blog/ http://www.stapp03.stratos.xfusioncorp.com:<port>/news/

</VirtualHost>
```

## Restart httpd service to apply changes in configuration files
```

systemctl restart httpd.service

systemctl status httpd.service

```


