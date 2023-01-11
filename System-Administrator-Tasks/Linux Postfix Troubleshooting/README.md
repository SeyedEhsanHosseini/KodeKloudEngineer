## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## Check the current status of mariadb service using systemctl

```
systemctl status postfix.service
```

## Try to start postfix using systemctl


```
systemctl start mariadb.service
```

#### You will get below output

```
Job for postfix.service failed because the control process exited with error code.
See "systemctl status postfix.service" and "journalctl -xe" for details.
```

## To see more detail on previous error use -l flag

```
systemctl status postfix.service -l
```

#### You will get below output

```
...

stmail01.stratos.xfusioncorp.com systemd[1]: Starting Postfix Mail Transport Agent...

stmail01.stratos.xfusioncorp.com aliasesdb[379]: /usr/sbin/postconf: fatal: parameter inet_interfaces: no local interface found for ::1

stmail01.stratos.xfusioncorp.com aliasesdb[379]: newaliases: fatal: parameter inet_interfaces: no local interface found for ::1

stmail01.stratos.xfusioncorp.com postfix/sendmail[381]: fatal: parameter inet_interfaces: no local interface found for ::1

stmail01.stratos.xfusioncorp.com postfix[383]: fatal: parameter inet_interfaces: no local interface found for ::1

stmail01.stratos.xfusioncorp.com systemd[1]: postfix.service: control process exited, code=exited status=1

stmail01.stratos.xfusioncorp.com systemd[1]: Failed to start Postfix Mail Transport Agent.

stmail01.stratos.xfusioncorp.com systemd[1]: Unit postfix.service entered failed state.

stmail01.stratos.xfusioncorp.com systemd[1]: postfix.service failed.

...

```

## Check the postfix main configuration file from below path and looking for "inet_interface" parameter

```
cat /etc/postfix/main.cf | grep inet_interface

```


#### You will get below output

```
...

# The inet_interfaces parameter specifies the network interface

inet_interfaces = all

#inet_interfaces = $myhostname

#inet_interfaces = $myhostname, localhost

inet_interfaces = localhost

# the address list specified with the inet_interfaces parameter.

# receives mail on (see the inet_interfaces parameter).

# to $mydestination, $inet_interfaces or $proxy_interfaces.

# - destinations that match $inet_interfaces or $proxy_interfaces,

# unknown@[$inet_interfaces] or unknown@[$proxy_interfaces] is returned

...

```

## Edit the postfix main configuration file from below path and comment "inet_interface" parameter


```
vi /etc/postfix/main.cf
```

```
inet_interfaces = localhost ==> #inet_interfaces = localhost
```

## Start and enable postfix service then check it's status using systemctl

```
systemctl start postfix.service

systemctl enable postfix.service

systemctl status postfix.service

```

