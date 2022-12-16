

## Login to the respective mentioned server in the task using ssh then switch to root user

```
thor@jump_host ~$ ssh natasha@ststor01
```
```
[natasha@ststor01 ~]$ sudo su -
```
## Run the below command to install Git
```
[root@ststor01 ~]# yum install -y git
```
#### Output:
```
Installed:
  git.x86_64 0:1.8.3.1-23.el7_8                                                                                                              

Dependency Installed:
  groff-base.x86_64 0:1.22.2-8.el7             less.x86_64 0:458-9.el7                      libedit.x86_64 0:3.0-12.20121213cvs.el7         
  openssh-clients.x86_64 0:7.4p1-22.el7_9      perl.x86_64 4:5.16.3-299.el7_9               perl-Carp.noarch 0:1.26-244.el7                 
  perl-Encode.x86_64 0:2.51-7.el7              perl-Error.noarch 1:0.17020-2.el7            perl-Exporter.noarch 0:5.68-3.el7               
  perl-File-Path.noarch 0:2.09-2.el7           perl-File-Temp.noarch 0:0.23.01-3.el7        perl-Filter.x86_64 0:1.49-3.el7                 
  perl-Getopt-Long.noarch 0:2.40-3.el7         perl-Git.noarch 0:1.8.3.1-23.el7_8           perl-HTTP-Tiny.noarch 0:0.033-3.el7             
  perl-PathTools.x86_64 0:3.40-5.el7           perl-Pod-Escapes.noarch 1:1.04-299.el7_9     perl-Pod-Perldoc.noarch 0:3.20-4.el7            
  perl-Pod-Simple.noarch 1:3.28-4.el7          perl-Pod-Usage.noarch 0:1.63-3.el7           perl-Scalar-List-Utils.x86_64 0:1.27-248.el7    
  perl-Socket.x86_64 0:2.010-5.el7             perl-Storable.x86_64 0:2.45-3.el7            perl-TermReadKey.x86_64 0:2.30-20.el7           
  perl-Text-ParseWords.noarch 0:3.29-4.el7     perl-Time-HiRes.x86_64 4:1.9725-3.el7        perl-Time-Local.noarch 0:1.2300-2.el7           
  perl-constant.noarch 0:1.27-2.el7            perl-libs.x86_64 4:5.16.3-299.el7_9          perl-macros.x86_64 4:5.16.3-299.el7_9           
  perl-parent.noarch 1:0.225-244.el7           perl-podlators.noarch 0:2.5.1-3.el7          perl-threads.x86_64 0:1.87-4.el7                
  perl-threads-shared.x86_64 0:1.43-6.el7      rsync.x86_64 0:3.1.2-11.el7_9               

Dependency Updated:
  openssh.x86_64 0:7.4p1-22.el7_9                                   openssh-server.x86_64 0:7.4p1-22.el7_9                                  

Complete!
```
## Create a bare repository /opt/ecommerce.git
```
[root@ststor01 ~]# cd /opt/
```
```
[root@ststor01 opt]# git init --bare ecommerce.git
```
#### Output:
```
Initialized empty Git repository in /opt/ecommerce.git/
```

