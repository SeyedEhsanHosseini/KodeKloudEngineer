## Login to the respective mentioned server in the task using ssh then switch to root user
```
ssh username@server

sudo su -
```

## First of all check for any available local yum repositories
```
yum repolist
```

#### Output:
```
Loaded plugins: fastestmirror, ovl
Determining fastest mirrors
repolist: 0
```

## Since there is no repo you need to create requested local repo 

```
Loaded plugins: fastestmirror, ovl
Determining fastest mirrors
repolist: 0
```

## Install requested package using yum package manager 

```
vi /etc/yum.repos.d/<repo-name>.repo
```
#### Write down these lines then save and exit
```
[repo-name]

name=repo-name

baseurl=file:///packages/downloaded_rpms/

enabled = 1

gpgcheck = 0

```
## Verify that your repository was successfully created

```
yum repolist
```

#### Output:
```
Loaded plugins: fastestmirror, ovl
Loading mirror speeds from cached hostfile
<repo-name>                                                                                                           | 2.9 kB  00:00:00     
<repo-name>/primary_db                                                                                                |  57 kB  00:00:00     
repo id                                                            repo name                                                          status
epel_local                                                         <repo-name>                                                         55
repolist: 55
```

## Use your local yum repository to install the requested package


```
yum install -y <package-name>
```

