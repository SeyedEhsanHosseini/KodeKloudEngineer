## create a user ‘john‘ with account expiry date 28th January 2021 in YYYY-MM-DD format
```
useradd -e 2021-01-28 john
```

## To verify
```
chage -l john
```

 
## Output:
```
Last password change                                    : Aug 29, 2022
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Jan 28, 2021
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```
