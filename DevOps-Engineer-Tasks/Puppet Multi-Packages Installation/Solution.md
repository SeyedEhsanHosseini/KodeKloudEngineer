## Create a puppet programming file games.pp under /etc/puppetlabs/code/environments/production/manifests on master node i.e on Jump Server 

```
thor@jump_host ~$ cd /etc/puppetlabs/code/environments/production/manifests/
thor@jump_host /etc/puppetlabs/code/environments/production/manifests$ sudo vi games.pp

```
#### Add the following lines, then save and exit.
```
class multi_package_node {

$multi_package = [ 'vim-enhanced', 'zip']

    package { $multi_package: ensure => 'installed' }

}

 

node 'stapp03.stratos.xfusioncorp.com' {

  include multi_package_node
}
```
## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Pull the configuration from the puppet server by running puppet agent
```
[root@stapp03 ~]# puppet agent -tv
```
```
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Retrieving locales
Info: Caching catalog for stapp03.stratos.xfusioncorp.com
Info: Applying configuration version '1669817820'
Notice: /Stage[main]/Multi_package_node/Package[vim-enhanced]/ensure: created
Notice: /Stage[main]/Multi_package_node/Package[zip]/ensure: created
Notice: Applied catalog in 29.51 seconds
```
## List installed packages and filter the list using grep to validate the task
```
[root@stapp03 ~]# rpm -qa | grep -e 'vim-enhanced' -e 'zip'
```
```
zip-3.0-11.el7.x86_64

vim-enhanced-7.4.629-8.el7_9.x86_64
```
