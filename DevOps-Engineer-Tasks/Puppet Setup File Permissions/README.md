## In the folder mentioned in the task, create a puppet file with the following content.

```
thor@jump_host ~$ vi /etc/puppetlabs/code/environments/production/manifests/ecommerce.pp
```

```
class file_modifier {
  file {'/opt/data/official.txt':
    ensure => 'present',
    content => 'Welcome to xFusionCorp Industries!',
    mode => '0644',
 }
}
node 'stapp03.stratos.xfusioncorp.com' {

   include file_modifier

}

``` 
 
## Use the following command to validate the puppet file

``` 
thor@jump_host ~$ puppet parser validate /etc/puppetlabs/code/environments/production/manifests/ecommerce.pp 
``` 

## Login to the respective mentioned server in the task using ssh then switch to root user

```
thor@jump_host ~$ ssh banner@stapp03
```

```
banner@stapp03's password: 
Last login: Thu Dec 15 16:18:56 2022 from jump_host.stratos.xfusioncorp.com
```
```
[banner@stapp03 ~]$ sudo su -
```
```
[sudo] password for banner: 
Last login: Thu Dec 15 16:19:07 UTC 2022 on pts/0
```

## Retrieve  the  client configuration from the Puppet master and apply it to the local host

``` 
[root@stapp03 ~]# puppet agent -tv
```
#### Output:
```
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Retrieving locales
Info: Caching catalog for stapp03.stratos.xfusioncorp.com
Info: Applying configuration version '1671121337'
Notice: /Stage[main]/File_modifier/File[/opt/data/official.txt]/content: 
--- /opt/data/official.txt      2022-12-15 16:10:51.077059680 +0000
+++ /tmp/puppet-file20221215-625-55ntsv 2022-12-15 16:22:18.501854824 +0000
@@ -0,0 +1 @@
+Welcome to xFusionCorp Industries!
\ No newline at end of file

Info: Computing checksum on file /opt/data/official.txt
Info: /Stage[main]/File_modifier/File[/opt/data/official.txt]: Filebucketed /opt/data/official.txt to puppet with sum d41d8cd98f00b204e9800998ecf8427e
Notice: /Stage[main]/File_modifier/File[/opt/data/official.txt]/content: content changed '{md5}d41d8cd98f00b204e9800998ecf8427e' to '{md5}b899e8a90bbb38276f6a00012e1956fe'
Notice: Applied catalog in 0.18 seconds
```
