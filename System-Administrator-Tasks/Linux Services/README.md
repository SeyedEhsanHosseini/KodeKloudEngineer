## Login to the respective mentioned server in the task using ssh

```
ssh username@server
```
## Install requested package using yum package manager

```
sudo yum install <package-name> -y
```
## Start and enable the package

```
sudo systemctl start <package-name>.service

sudo systemctl enable <package-name>.service

```

## Check the status of service for validation

```
sudo systemctl status <package-name>.service

```
