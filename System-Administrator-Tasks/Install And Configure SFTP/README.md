## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## Create the SFTP user and set the desired password for it
```
useradd <username>
```
```
passwd <username>
```
#### Output:
```
Changing password for user <username>.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

#### Create the Chroot directory and set its ownership and permissions
```
mkdir -p /var/www/<directory>
```
```
chown root:root /var/www/
```
```
chmod -R 755 /var/www/
```

## Edit sshd_config file
```
vi /etc/ssh/sshd_config
```
#### Edit last section of config from this:
```
# override default of no subsystems
Subsystem       sftp    /usr/libexec/openssh/sftp-server

# Example of overriding settings on a per-user basis
#Match User anoncvs
#       X11Forwarding no
#       AllowTcpForwarding no
#       PermitTTY no
#       ForceCommand cvs server
```
#### To be like this
```
Subsystem       sftp    internal-sftp

Match User <username>

ForceCommand internal-sftp

PasswordAuthentication yes

ChrootDirectory /var/www/<directory>

PermitTunnel no

AllowTcpForwarding no

X11Forwarding no

AllowAgentForwarding no
```

## Restart & check the service status 
```
systemctl restart sshd && systemctl status sshd
```
#### Output:
```
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2022-11-05 17:56:48 UTC; 6ms ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 689 (sshd)
   CGroup: /docker/75d2449b056aae0f21489f661ceb8ac8f7d2bc639ceaffa82910f025132a7261/system.slice/sshd.service
           ├─609 sshd: tony [priv]
           ├─618 sshd: tony@pts/0
           ├─619 -bash
           └─689 /usr/sbin/sshd -D

Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com systemd[689]: Executing: /usr/sbin/sshd -D
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com sshd[689]: WARNING: 'UsePAM no' is not supported in Red Hat Enterprise Linux and may cause several problems.
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com sshd[689]: Server listening on 0.0.0.0 port 22.
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com sshd[689]: Server listening on :: port 22.
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com systemd[1]: Got notification message for unit sshd.service
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com systemd[1]: sshd.service: Got notification message from PID 689 (READY=1)
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com systemd[1]: sshd.service: got READY=1
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com systemd[1]: sshd.service changed start -> running
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com systemd[1]: Job sshd.service/start finished, result=done
Nov 05 17:56:48 stapp01.stratos.xfusioncorp.com systemd[1]: Started OpenSSH server daemon.
```


## Validate the task
```
sftp <username>@localhost
```

#### Output must be like this:
```
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:dNdO5b3wQ0lOWWS/XVqhvPTIHIsvWRblbCpwxOUUkN0.
ECDSA key fingerprint is MD5:69:76:6f:af:57:67:94:a8:c8:d5:4c:34:73:e8:79:71.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
<username>@localhost's password: 
Connected to localhost.
sftp> 
```



















