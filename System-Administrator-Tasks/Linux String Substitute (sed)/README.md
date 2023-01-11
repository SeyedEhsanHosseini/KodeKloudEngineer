## Login to the respective mentioned server in the task using ssh

```
ssh username@server
```
## Checking the contents of the desired file before making changes

```
cat /path/to/<file_name>
```
## Apply the desired changes to the file

```
sudo sed '/word/d' /path/to/<file_name> /path/to/<file_name_DELETE>
```

```
sudo sed 's/\<word\>/replace/g' /path/to/<file_name> /path/to/<file_name_REPLACE>
```

## To verify the changes we can use grep commnad

```
grep "word" /path/to/<file_name>

grep "word" /path/to/<file_name_DELETE>

grep "word" /path/to/<file_name_REPLACE>
```
