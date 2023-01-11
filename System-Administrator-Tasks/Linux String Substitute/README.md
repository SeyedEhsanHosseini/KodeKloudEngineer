## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## Checking the contents of the desired file before making changes

```
grep "word" /path/to/<file_name>
```
## Apply the desired changes to the file

```
sudo sed -i 's/\<word\>/replace/g' /path/to/<file_name>
```

## To verify the changes we can use grep commnad

```
grep "word" /path/to/<file_name>

grep "replace" /path/to/<file_name>

```
