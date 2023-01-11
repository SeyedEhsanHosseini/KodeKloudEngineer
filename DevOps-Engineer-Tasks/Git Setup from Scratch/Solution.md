
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
  perl-threads-shared.x86_64 0:1.43-6.el7      rsync.x86_64 0:3.1.2-12.el7_9

Dependency Updated:
  openssh.x86_64 0:7.4p1-22.el7_9                                   openssh-server.x86_64 0:7.4p1-22.el7_9

Complete!
```

## set up any values for user.email and user.name globally  create a bare repository /opt/apps.git.
```
[root@ststor01 ~]# git config --global --add user.name natasha

[root@ststor01 ~]# git config --global --add user.email natasha@ststor01.stratos.xfusioncorp.com
```

## Create a bare repository /opt/beta.git.
```
[root@ststor01 ~]# git init --bare /opt/beta.git
```
#### Output:
```
Initialized empty Git repository in /opt/beta.git/
```

## There is an update hook (to block direct pushes to master branch) under /tmp on storage server itself; use the same to block direct pushes to master branch in /opt/beta.git repo.
```
[root@ststor01 ~]# cp /tmp/update /opt/beta.git/hooks/
```

## Clone /opt/beta.git repo in /usr/src/kodekloudrepos/beta directory.
```
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/

[root@ststor01 kodekloudrepos]# git clone /opt/beta.git
```
#### Output:
```
Cloning into 'beta'...
warning: You appear to have cloned an empty repository.
done.
```

## Create a new branch xfusioncorp_beta in repo that you cloned in /usr/src/kodekloudrepos.
```
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/

[root@ststor01 kodekloudrepos]# cd beta/

[root@ststor01 beta]# git checkout -b xfusioncorp_beta
```
#### Output:
```
Switched to a new branch 'xfusioncorp_beta'
```

## There is a readme.md file in /tmp on storage server itself; copy that to repo, add/commit in the new branch you created
```
[root@ststor01 beta]# cp /tmp/readme.md .

[root@ststor01 beta]# git add readme.md 

[root@ststor01 beta]# git commit -m "add readme.md file"
```
#### Output:
```
[xfusioncorp_beta (root-commit) 4cace74] add readme.md file
 1 file changed, 1 insertion(+)
 create mode 100644 readme.md
``` 

## Finally push your branch to origin

```
[root@ststor01 beta]# git push origin xfusioncorp_beta
```
#### Output: 
```
Counting objects: 3, done.
Writing objects: 100% (3/3), 258 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To /opt/beta.git
 * [new branch]      xfusioncorp_beta -> xfusioncorp_beta
```  
## create master branch from your branch and remember you should not be able to push to master as per hook you have set up.

``` 
[root@ststor01 beta]# git checkout -b master
```
#### Output: 
```
Switched to a new branch 'master'
[root@ststor01 beta]# git branch
* master
  xfusioncorp_beta
``` 
### Push to master  
```
[root@ststor01 beta]# git push origin master
```
#### Output: 

```
Total 0 (delta 0), reused 0 (delta 0)
remote: Manual pushing to this repo's master branch is restricted
remote: error: hook declined to update refs/heads/master
To /opt/beta.git
 ! [remote rejected] master -> master (hook declined)
error: failed to push some refs to '/opt/beta.git'
```











