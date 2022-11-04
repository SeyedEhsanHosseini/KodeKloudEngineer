## Copy ROOT.war file to the app server using scp

```
scp /tmp/ROOT.war banner@stapp03:/tmp
```

## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Install tomcat

```
yum install -y tomcat
```

## Edit tomcat configuration file from below path
```
vi /usr/share/tomcat/conf/server.xml
```
#### add your desired port instead of <port> then save the file & exit
```
    Define a non-SSL HTTP/1.1 Connector on port 8080
    -->
    <Connector port="<port>" protocol="HTTP/1.1"        
               connectionTimeout="20000"
               redirectPort="8443" />
```
## Copy ROOT.war file to the below path
```
cp /tmp/ROOT.war /usr/share/tomcat/webapps
```

## Start and enable Tomcat service
```
systemctl start tomcat && systemctl enable tomcat
```
#### Output:
```
Created symlink from /etc/systemd/system/multi-user.target.wants/tomcat.service to /usr/lib/systemd/system/tomcat.service.
```
#### then check its status

```
systemctl status tomcat
```

#### Output:
```
● tomcat.service - Apache Tomcat Web Application Container
   Loaded: loaded (/usr/lib/systemd/system/tomcat.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2022-11-04 09:29:00 UTC; 6s ago
 Main PID: 1176 (java)
   CGroup: /docker/dcde20f0fb1f620df09b511ba0a4f1ae8b197ec2cf801a865194d8dc2206f4bb/system.slice/tomcat.service
           └─1176 /usr/lib/jvm/jre/bin/java -Djavax.sql.DataSource.Factory=org.apache.commons.dbcp.BasicDataSourceFactory -classpath /usr/share/tomcat/bin/b...

Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: Nov 04, 2022 9:29:05 AM org.apache.catalina.startup.TldConfig execute
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: INFO: At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging...n time.
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: Nov 04, 2022 9:29:05 AM org.apache.catalina.startup.HostConfig deployWAR
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: INFO: Deployment of web application archive /var/lib/tomcat/webapps/ROOT.war has finish...,392 ms
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: Nov 04, 2022 9:29:05 AM org.apache.coyote.AbstractProtocol start
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: INFO: Starting ProtocolHandler ["http-bio-6300"]
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: Nov 04, 2022 9:29:05 AM org.apache.coyote.AbstractProtocol start
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: INFO: Starting ProtocolHandler ["ajp-bio-8009"]
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: Nov 04, 2022 9:29:05 AM org.apache.catalina.startup.Catalina start
Nov 04 09:29:05 stapp03.stratos.xfusioncorp.com server[1176]: INFO: Server startup in 2596 ms
Hint: Some lines were ellipsized, use -l to show in full.
```

## Validate the task 
```
curl -i http://<app-server>:<port>
```

#### Output must be like this:
```
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Accept-Ranges: bytes
ETag: W/"471-1580289830000"
Last-Modified: Wed, 29 Jan 2020 09:23:50 GMT
Content-Type: text/html
Content-Length: 471
Date: Fri, 04 Nov 2022 09:29:49 GMT

<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html>
    <head>
        <title>SampleWebApp</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
    </head>
    <body>
        <h2>Welcome to xFusionCorp Industries!</h2>
        <br>
    
    </body>
</html>
```

 





























