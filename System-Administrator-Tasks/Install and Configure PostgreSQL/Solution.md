## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## Install PostgreSQL

```
yum install -y postgresql-server postgresql-contrib
```
## Initiate DB setup  

```
postgresql-setup initdb
```
#### Output:
```
Initializing database ... OK
```

## Start, enable & check the service status 
```
systemctl start postgresql && systemctl enable postgresql
```
#### Output:
```
Created symlink from /etc/systemd/system/multi-user.target.wants/postgresql.service to /usr/lib/systemd/system/postgresql.service.
[root@stdb01 ~]# systemctl status postgresql
● postgresql.service - PostgreSQL database server
   Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2022-11-02 17:32:49 UTC; 35s ago
 Main PID: 1074 (postgres)
   CGroup: /docker/04b5a8ae7da910532be1cb332b44a7ff0b79d54f1391ea7b252738511bbf82b7/system.slice/postgresql.service
           ├─1074 /usr/bin/postgres -D /var/lib/pgsql/data -p 5432
           ├─1075 postgres: logger process   
           ├─1077 postgres: checkpointer process   
           ├─1078 postgres: writer process   
           ├─1079 postgres: wal writer process   
           ├─1080 postgres: autovacuum launcher process   
           └─1081 postgres: stats collector process   

Nov 02 17:32:48 stdb01.stratos.xfusioncorp.com systemd[1]: Starting PostgreSQL database server...
Nov 02 17:32:48 stdb01.stratos.xfusioncorp.com pg_ctl[1072]: LOG:  could not bind IPv6 socket: Cannot assign requested address
Nov 02 17:32:48 stdb01.stratos.xfusioncorp.com pg_ctl[1072]: HINT:  Is another postmaster already running on port 5432? If not, wait a few seconds and retry.
Nov 02 17:32:49 stdb01.stratos.xfusioncorp.com systemd[1]: Started PostgreSQL database server.
```

## Now login to Postgres, then create user , create database 
## and grant all privileges on database to the created user

```
[root@stdb01 ~]# sudo -u postgres psql
```
#### Output:
```
could not change directory to "/root"
psql (9.2.24)
Type "help" for help.
```

#### CREATE USER
```
postgres=# CREATE USER <DB_username> WITH PASSWORD '<DB_password>';
```
#### Output:
```
CREATE ROLE
```
#### CREATE DATABASE
```
postgres=# CREATE DATABASE <DB_name>;
```
#### Output:
```
CREATE DATABASE
```
#### GRANT ALL PRIVILEGES
```
postgres=# GRANT ALL PRIVILEGES ON DATABASE "<DB_name>" to "<DB_username>";
```
#### Output:
```
GRANT
```
#### to Quit:
```
postgres=# \q
```

## Edit postgresql.conf
```
vi /var/lib/pgsql/data/postgresql.conf
```
#### Uncomment the "listen_addresses" variable then save & exit
```
#------------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#------------------------------------------------------------------------------

# - Connection Settings -

listen_addresses = 'localhost'          # what IP address(es) to listen on;
```
## Edit pg_hba.conf  
```
vi /var/lib/pgsql/data/pg_hba.conf
```
#### Edit config from this
``` 

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident

```
#### To be like this

```

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5

```
## Restart PostgreSQL service and check the service status
```
systemctl restart postgresql && systemctl status postgresql
```
#### Output:
```
● postgresql.service - PostgreSQL database server
   Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2022-11-02 17:43:14 UTC; 7s ago
  Process: 1237 ExecStop=/usr/bin/pg_ctl stop -D ${PGDATA} -s -m fast (code=exited, status=0/SUCCESS)
  Process: 1243 ExecStart=/usr/bin/pg_ctl start -D ${PGDATA} -s -o -p ${PGPORT} -w -t 300 (code=exited, status=0/SUCCESS)
  Process: 1238 ExecStartPre=/usr/bin/postgresql-check-db-dir ${PGDATA} (code=exited, status=0/SUCCESS)
 Main PID: 1245 (postgres)
   CGroup: /docker/04b5a8ae7da910532be1cb332b44a7ff0b79d54f1391ea7b252738511bbf82b7/system.slice/postgresql.service
           ├─1245 /usr/bin/postgres -D /var/lib/pgsql/data -p 5432
           ├─1246 postgres: logger process   
           ├─1248 postgres: checkpointer process   
           ├─1249 postgres: writer process   
           ├─1250 postgres: wal writer process   
           ├─1251 postgres: autovacuum launcher process   
           └─1252 postgres: stats collector process   

Nov 02 17:43:13 stdb01.stratos.xfusioncorp.com systemd[1]: Starting PostgreSQL database server...
Nov 02 17:43:13 stdb01.stratos.xfusioncorp.com pg_ctl[1243]: LOG:  could not bind IPv6 socket: Cannot assign requested address
Nov 02 17:43:13 stdb01.stratos.xfusioncorp.com pg_ctl[1243]: HINT:  Is another postmaster already running on port 5432? If not, wait a few seconds and retry.
Nov 02 17:43:14 stdb01.stratos.xfusioncorp.com systemd[1]: Started PostgreSQL database server.
```

## Validate the task by running below commands
```
psql -U <DB_username> -d <DB_name> -h 127.0.0.1 -W

psql -U <DB_username> -d <DB_name> -h localhost -W
```

#### Output must be like this:
```
Password for user kodekloud_cap: 
psql (9.2.24)
Type "help" for help.

kodekloud_db8=>
```





