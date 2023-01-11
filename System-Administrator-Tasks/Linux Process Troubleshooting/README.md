## By running the below commands, you can identify the app server that has the issue

#### For app server 1
```
telnet stapp01 <port>
```
#### Output:
```
Trying 172.16.238.10... 
telnet: connect to address 172.16.238.10: Connection refused 
```
#### For app server 2
```
telnet stapp02 <port>
```
#### Output:
```
telnet stapp02 <port> Trying 172.16.238.11... 
Connected to stapp02.
```
#### For app server 3
```
telnet stapp03 <port>
```
#### Output:
```
telnet stapp03 <port> Trying 172.16.238.12... 
Connected to stapp03.
```
## Login to that app server which has connection refuse error using SSH then switch to the root user
```
ssh username@server

sudo su -
```
## Check httpd service status and try to start it using systemctl

```
systemctl start httpd 
```
#### Output:
```
Job for httpd.service failed because the control process exited with error code.
 See "systemctl status httpd.service" and "journalctl -xe" for details. 
```
```
systemctl status httpd 
```
#### Output:
```
● httpd.service - The Apache HTTP Server Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled;
vendor preset: disabled) 
Active: failed (Result: exit-code) since Tue 2022-11-01 15:20:45 UTC;
7s ago Docs: man:httpd(8) man:apachectl(8) 
Process: 704 ExecStop=/bin/kill -WINCH ${MAINPID} (code=exited, status=1/FAILURE)
Process: 703 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE) 
Main PID: 703 (code=exited, status=1/FAILURE)
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com httpd[703]: AH00558: httpd: Could not reliably determine the server's fully qualified domain name, us...message 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com httpd[703]: (98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:<port> 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com httpd[703]: no listening sockets available, shutting down 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com httpd[703]: AH00015: Unable to open logs 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: main process exited, code=exited, status=1/FAILURE 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com kill[704]: kill: cannot find process "" 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: control process exited, code=exited status=1 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com systemd[1]: Failed to start The Apache HTTP Server. 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com systemd[1]: Unit httpd.service entered failed state. 
Nov 01 15:20:45 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service failed. Hint: Some lines were ellipsized, use -l to show in full. 
```
#### On the 9th line, you can see that another service is already using the port number assigned to httpd

## Netstat will show you which service is using your port and what its PID is

```
netstat -tulnp 
```
#### Output:
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/init 
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      530/sshd 
tcp        0      0 127.0.0.11:37879        0.0.0.0:*               LISTEN      - 
tcp        0      0 127.0.0.1:<port>        0.0.0.0:*               LISTEN      <PID>/sendmail:  
tcp6       0      0 :::111                  :::*                    LISTEN      511/rpcbind 
tcp6       0      0 :::22                   :::*                    LISTEN      530/sshd 
udp        0      0 127.0.0.11:55907        0.0.0.0:*                           - 
udp        0      0 0.0.0.0:111             0.0.0.0:*                           1/init 
udp        0      0 0.0.0.0:681             0.0.0.0:*                           511/rpcbind 
udp6       0      0 :::111                  :::*                                511/rpcbind 
udp6       0      0 :::681                  :::*                                511/rpcbind 
```
## Use the kill signal and the PID of the service that is using the httpd port to terminate it

```
kill <PID> 
```
## Try to start httpd service again then check its status
``` 
systemctl start httpd && systemctl status httpd 
```
#### Output:
```
● httpd.service - The Apache HTTP Server Loaded: loaded (/usr/lib/systemd/system/httpd.service;
disabled; vendor preset: disabled) 
Active: active (running) since Tue 2022-11-01 15:22:55 UTC;
7s ago Docs: man:httpd(8) man:apachectl(8) Process: 
704 ExecStop=/bin/kill -WINCH ${MAINPID} (code=exited, status=1/FAILURE) Main PID: 
749 (httpd) Status: "Processing requests..." CGroup: /docker/99352f03935f57686ecf2dbc33c3b51adaa844a1f6c39b2ae33709b5010a2655/system.slice/httpd.service 
├─749 /usr/sbin/httpd -DFOREGROUND 
├─751 /usr/sbin/httpd -DFOREGROUND 
|─752 /usr/sbin/httpd -DFOREGROUND 
├─753 /usr/sbin/httpd -DFOREGROUND 
├─754 /usr/sbin/httpd -DFOREGROUND 
└─755 /usr/sbin/httpd -DFOREGROUND 
Nov 01 15:22:54 stapp01.stratos.xfusioncorp.com systemd[1]: Starting The Apache HTTP Server... 
Nov 01 15:22:54 stapp01.stratos.xfusioncorp.com httpd[749]: AH00558: httpd: Could not reliably determine the server's fully qualified domain name, us...message 
Nov 01 15:22:55 stapp01.stratos.xfusioncorp.com systemd[1]: Started The Apache HTTP Server. Hint: Some lines were ellipsized, use -l to show in full. 
```


