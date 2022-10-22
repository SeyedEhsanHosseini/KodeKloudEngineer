## Login to the respective mentioned server in the task using ssh then switch to root user
```
ssh username@server

sudo su -
```

## To hide web server version number, server operating system details, installed Apache modules and more,
## Edit Apache web server configuration file using "vi" and add the lines below then save and exit

```
vi /etc/httpd/conf/httpd.conf
```

#### Content:

```
ServerTokens Prod
ServerSignature Off 

```

## restart your Apache web server like so

```
systemctl restart httpd.service
```

## To disable Apache directory listing via Directory's Options directive,
## Edit Apache web server configuration file using "vi" and add the lines below then save and exit 

```
vi /etc/httpd/conf/httpd.conf
```

#### Content:
###### Add -Indexes to Options directive for required directory:
```
<Directory /var/www/html/official>
    Options -Indexes
</Directory>
```

## restart your Apache web server like so

```
systemctl restart httpd.service
```
