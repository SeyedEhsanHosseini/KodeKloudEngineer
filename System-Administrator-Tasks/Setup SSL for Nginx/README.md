## Login to the respective mentioned server in the task using ssh then switch to root user
```
ssh username@server

sudo su -
```

## Install Nginx webserver

#### Adding the EPEL Software Repository

```
yum install -y epel-release
```

#### Installing Nginx

```
yum install -y nginx
```

#### Start and Enable Nginx service
```

systemctl start nginx.service

systemctl enable nginx.service

```

## Edit Nginx configuration file like this, then save and exit

```
vi /etc/nginx/nginx.conf
```

#### Content:
```
server {

        listen       80;

        listen       [::]:80;

        server_name  <app_server_ip>;

        root         /usr/share/nginx/html;

 

        # Load configuration files for the default server block.

        include /etc/nginx/default.d/*.conf;

 

        error_page 404 /404.html;

        location = /404.html {

        }

 

        error_page 500 502 503 504 /50x.html;

        location = /50x.html {

        }

    }

 

# Settings for a TLS enabled server.

 

    server {

        listen       443 ssl http2;

        listen       [::]:443 ssl http2;

        server_name  <app_server_ip>;

        root         /usr/share/nginx/html;

        ssl_certificate "/etc/pki/CA/certs/nautilus.crt";

        ssl_certificate_key "/etc/pki/CA/private/nautilus.key";

        ssl_session_cache shared:SSL:1m;

        ssl_session_timeout  10m;

        ssl_ciphers HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers on;

 

#        # Load configuration files for the default server block.

        include /etc/nginx/default.d/*.conf;

 

        error_page 404 /404.html;

            location = /40x.html {

        }

 

        error_page 500 502 503 504 /50x.html;

            location = /50x.html {

        }

    }

 

}

```

## Copy SSL certificate and key to below path

```
cp /tmp/nautilus.crt /etc/pki/CA/certs/

cp /tmp/nautilus.key /etc/pki/CA/private/
```

## Remove previous index.html file from document root then Create a new index.html file with "Welcome!" on Nginx document root 

```
rm /usr/share/nginx/html/index.html

vi /usr/share/nginx/html/index.html

```

## Restart nginx service to apply changes in configuration file
```

systemctl restart nginx.service

systemctl status nginx.service

```

## Validate the task from Jump server

```
curl -Ik https://<app_server_ip>
```

#### Output:

```
HTTP/1.1 200 OK

Server: nginx/1.20.1

Date: Fri, 21 Oct 2022 16:32:39 GMT

Content-Type: text/html

Content-Length: 9

Last-Modified: Fri, 21 Oct 2022 16:14:24 GMT

Connection: keep-alive

ETag: "60fd55a0-9"

Accept-Ranges: bytes
```
