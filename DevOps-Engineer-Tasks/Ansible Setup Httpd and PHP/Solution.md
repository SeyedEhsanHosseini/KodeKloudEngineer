## Change current working directory to the folder mentioned in task to list all the files then create a playbook
```
thor@jump_host ~$ cd ~/playbooks/

thor@jump_host ~/playbooks$ ls -l
```
#### Output:
```
total 12
-rw-r--r-- 1 thor thor   36 Dec 18 16:43 ansible.cfg
-rw-r--r-- 1 thor thor  237 Dec 18 16:43 inventory
drwxr-xr-x 2 thor thor 4096 Dec 18 16:43 templates
```

## Create a playbook file named httpd.yml with the following content
```
thor@jump_host ~/playbooks$ vi httpd.yml
```

```
---
- name: Install Httpd and PHP Packages

  hosts: stapp03

  become: yes

  tasks:

    - name: Install latest version of httpd and php

      package:

        name:

          - httpd

          - php

        state: latest

    - name: Replace default DocumentRoot in httpd.conf

      replace:

        path: /etc/httpd/conf/httpd.conf

        regexp: DocumentRoot \"\/var\/www\/html\"

        replace: DocumentRoot "/var/www/html/myroot"

    - name: Create the new DocumentRoot directory if it does not exist

      file:

        path: /var/www/html/myroot

        state: directory

        owner: apache

        group: apache

    - name: Use Jinja2 template to generate phpinfo.php

      template:

        src: /home/thor/playbooks/templates/phpinfo.php.j2

        dest: /var/www/html/myroot/phpinfo.php

        owner: apache

        group: apache

    - name: Start and enable service httpd

      service:

        name: httpd

        state: started

        enabled: yes 
``` 
       
## To execute the playbook, run the following command
  
      
```     
thor@jump_host ~/playbooks$ ansible-playbook -i inventory httpd.yml 
```
#### Output:
```
PLAY [Install Httpd and PHP Packages] *******************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [stapp03]

TASK [Install latest version of httpd and php] **********************************************************************************************
changed: [stapp03]

TASK [Replace default DocumentRoot in httpd.conf] *******************************************************************************************
changed: [stapp03]

TASK [Create the new DocumentRoot directory if it does not exist] ***************************************************************************
changed: [stapp03]

TASK [Use Jinja2 template to generate phpinfo.php] ******************************************************************************************
changed: [stapp03]

TASK [Start and enable service httpd] *******************************************************************************************************
changed: [stapp03]

PLAY RECAP **********************************************************************************************************************************
stapp03                    : ok=6    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  

```




      
        
        
        
        
