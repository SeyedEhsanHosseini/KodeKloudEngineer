## Login to the respective mentioned server in the task using ssh then switch to root user
```
ssh username@server

sudo su -
```

## Install Apache webserver using yum package manager 
```
yum install -y httpd
```


## Edit Apache configuration, change port number,  enable below mentioned headers, then save and exit

```
vi /etc/httpd/conf/httpd.conf
```

#### Content:
```

Listen <port>

Header set X-XSS-Protection "1; mode=block"

Header always append X-Frame-Options SAMEORIGIN

Header set X-Content-Type-Options nosniff

```

##  Create an html file in /var/www/html/ path with below content

```
vi /var/www/html/index.html
```
#### Content:
```
Welcome to the xFusionCorp Industries!
```

## Start, Enable and Restart httpd service to apply changes in configuration files
```
systemctl start httpd.service

systemctl enable httpd.service

systemctl restart httpd.service

```

## Validate task using curl command 

```
curl http://localhost:<port>
```
#### Output:
```
Welcome to the xFusionCorp Industries!
```
### Step 2:
```
curl -i http://localhost:<port>
```
#### Output:
```
HTTP/1.1 200 OK

Date: Tue, 18 Oct 2021 07:32:26 GMT

Server: Apache/2.4.6 (CentOS)

X-Frame-Options: SAMEORIGIN

Last-Modified: Tue, 18 Oct 2021 07:30:12 GMT

ETag: "27-5c74cb2316a2c"

Accept-Ranges: bytes

Content-Length: 39

X-XSS-Protection: 1; mode=block

X-Content-Type-Options: nosniff

Content-Type: text/html; charset=UTF-8

 Welcome to the xFusionCorp Industries!
```


