## Login to the respective mentioned server in the task using ssh

```
ssh username@server
```
## Install SELinux components in CentOS

```
yum install -y policycoreutils policycoreutils-python selinux-policy selinux-policy-targeted libselinux-utils setroubleshoot-server setools setools-console mcstrans
```
## To check the SELinux status, run the below command

```
sestatus
```

## To configure, edit the SELinux configuration file /etc/sysconfig/selinux.

```
vi /etc/sysconfig/selinux
```

```
SELINUX=permissive
OR
SELINUX=enforcing
OR
SELINUX=disabled

```
