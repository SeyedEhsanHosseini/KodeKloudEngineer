## Switch to the root user on the master node 

thor@jump_host ~$ sudo su -

## List  all the puppet modules which have been installed:

root@jump_host  puppet module list
/opt/puppetlabs/puppet/modules (no modules installed)
 
## Install the puppetlabs-ntp module::

root@jump_host ~$ puppet module install puppetlabs-ntp

#### Output:

Notice: Preparing to install into /home/thor/.puppetlabs/etc/code/modules ...
Notice: Created target directory /home/thor/.puppetlabs/etc/code/modules
Notice: Downloading from https://forgeapi.puppet.com ...
Notice: Installing -- do not interrupt ...
/home/thor/.puppetlabs/etc/code/modules
└─┬ puppetlabs-ntp (v9.2.0)
  └── puppetlabs-stdlib (v8.5.0)
  
  
## Create a puppet file under the path mentioned in the task and add the following lines to it then save and exit 
  
```  
vi /etc/puppetlabs/code/environments/production/manifests/beta.pp
```
```
class { 'ntp':

  servers => [ '2.cn.pool.ntp.org iburst' ],

}



class ntpconfig {

  include ntp

}


node 'stapp02.stratos.xfusioncorp.com' {

  include ntpconfig

}
```
## By manually running `puppet parser validate` make sure that the manifest can be parsed before you commit your changes or deploy them to a live environment 
```
puppet parser validate /etc/puppetlabs/code/environments/production/manifests/beta.pp
```

## Login to the respective mentioned server in the task using ssh then switch to root user

```
root@jump_host ~# ssh steve@stapp02
```
```
[steve@stapp03 ~]$ sudo su -
```

## Check the current puppet service resource status

```
[root@stapp02 ~]# puppet resource service ntpd
```
```
service { 'ntpd':
  ensure   => 'stopped',
  enable   => 'false',
  provider => 'systemd',
}
```

## Pull puppet configuration from the master node
```
[root@stapp02 ~]# puppet agent -tv
```
#### Output: 
```
Notice: Run of Puppet configuration client already in progress; skipping  (/opt/puppetlabs/puppet/cache/state/agent_catalog_run.lock exists)
```

### Stop the puppet service on the app server then try again 
```
[root@stapp02 ~]# systemctl stop puppet
```
```
[root@stapp02 ~]# puppet agent -tv
```

#### Output:
```
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Retrieving locales
Info: Loading facts
Info: Caching catalog for stapp02.stratos.xfusioncorp.com
Info: Applying configuration version '1671661733'
Notice: Applied catalog in 0.15 seconds
```
## Start puppet service then check the current puppet service resource status:
```
[root@stapp02 ~]# systemctl start puppet
```
```
[root@stapp02 ~]# puppet resource service ntpd
```
#### Output:
```
service { 'ntpd':
  ensure   => 'running',
  enable   => 'true',
  provider => 'systemd',
}
```