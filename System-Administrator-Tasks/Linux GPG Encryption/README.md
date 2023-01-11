## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```
## List all files located in /home directory 

```
ls -l
```
#### Output:
```
total 24
drwx------ 1 ansible ansible 4096 Oct 15  2019 ansible
-rw-r--r-- 1 root    root     155 Oct 25 18:12 decrypt_me.asc
-rw-r--r-- 1 root    root      99 Oct 25 18:15 encrypt_me.txt
drwx------ 1 natasha natasha 4096 Jan 12  2020 natasha
-rw-r--r-- 1 root    root    3589 Oct 25 18:15 private_key.asc
-rw-r--r-- 1 root    root    1722 Oct 25 18:15 public_key.asc
```
## Import public key 

```
gpg --import public_key.asc
```
#### Output:
```
gpg: /root/.gnupg/trustdb.gpg: trustdb created
gpg: key CCE3AF51: public key "kodekloud <kodekloud@kodekloud.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
```
## Import private key 
```
gpg --import private_key.asc
```
#### Output:
```
gpg: key CCE3AF51: secret key imported
gpg: key CCE3AF51: "kodekloud <kodekloud@kodekloud.com>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1
```
## Validate imported keys

```
gpg --list-keys
```
#### Output:
```
/root/.gnupg/pubring.gpg
------------------------
pub   2048R/CCE3AF51 2020-01-19
uid                  kodekloud <kodekloud@kodekloud.com>
sub   2048R/865C070D 2020-01-19
```
```
gpg --list-secret-keys
```
#### Output:
```
/root/.gnupg/secring.gpg
------------------------
sec   2048R/CCE3AF51 2020-01-19
uid                  kodekloud <kodekloud@kodekloud.com>
ssb   2048R/865C070D 2020-01-19
```

## Encrypt the "encrypt_me.txt" file 
```
gpg --encrypt -r kodekloud@kodekloud.com --armor < encrypt_me.txt -o encrypted_me.asc
```
#### Output:
```
gpg: 865C070D: There is no assurance this key belongs to the named user

pub  2048R/865C070D 2020-01-19 kodekloud <kodekloud@kodekloud.com>
 Primary key fingerprint: FEA8 5011 C456 B5E9 AE5A  516F 8F17 F26E CCE3 AF51
      Subkey fingerprint: 7B4B 5CFC 5E4F B4B6 EEC0  83E5 DD6B 8506 865C 070D

It is NOT certain that the key belongs to the person named
in the user ID.  If you *really* know what you are doing,
you may answer the next question with yes.

Use this key anyway? (y/N) y
```
## Decrypt the "encrypt_me.txt" file using "kodekloud" as passphrase
```
gpg --decrypt decrypt_me.asc > decrypted_me.txt
```
#### Output:
```
gpg: AES encrypted data
gpg: encrypted with 1 passphrase
```

