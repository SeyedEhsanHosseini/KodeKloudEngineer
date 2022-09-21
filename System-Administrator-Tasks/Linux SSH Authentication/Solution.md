## Generate a key pair on localhost

```
ssh-keygen -t ed25519
```

#### Output

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/thor/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase): Enter same passphrase again:
Your identification has been saved in /home/thor/.ssh/id_ed25519.
Your public key has been saved in /home/thor/.ssh/id_ed25519.pub.
The key fingerprint is: SHA256:sSQYkL7hcKe8Qo/vxQKGkFGr4KVodOi5091ylZHqDK8
thor@jump_host.stratos.xfusioncorp.com The key's randomart image is:
 +--[ED25519 256]--+ 
 |..oo.            | 
 | oo. o    .      | 
 |++.o. . oo       | 
 |X.O .  o.oo      | 
 |+& = . .So       | 
 |o.O o * .        | 
 |.oo+ = *         | 
 |..o.o +          | 
 | .oo E           | 
 +----[SHA256]-----+
 
```


## Upload public key to remote server

```  
ssh-copy-id username@remoteserver
``` 

#### Output

```
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_ed25519.pub" 
The authenticity of host 'remoteserver (172.16.238.10)' can't be established. 
ECDSA key fingerprint is SHA256:/s8E7fwhDMI4HleH8fCcaqdEO79u0qDQDZno71jUUDY. 
ECDSA key fingerprint is MD5:07:42:6f:3a:08:11:a7:6d:c4:2d:6b:9b:76:32:21:84. 
Are you sure you want to continue connecting (yes/no)? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed /bin/ssh-copy-id: 
INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys username@remoteserver's password: Number of key(s) added: 1 
Now try logging into the machine, with:   "ssh 'username@remoteserver'" and check to make sure that only the key(s) you wanted were added.

```

