## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Install requested package using yum package manager

```
yum install cronie -y
```
## Start and enable the service

```
systemctl start crond.service

systemctl enable crond.service

```

## Open the crontab configuration file for the current user

crontab -e


#### You can add any number of scheduled tasks, one per line.
#### Once you have finished adding tasks, save the file and exit.
#### The cron daemon will read and execute the instructions provided.
#### Remember, Cron does not need to be restarted to apply changes.

## Output:
```
crontab: installing new crontab
```
