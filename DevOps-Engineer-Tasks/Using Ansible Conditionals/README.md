## Change the working directory to the Ansible directory and list its files 
```
thor@jump_host ~$ cd /home/thor/ansible/
thor@jump_host ~/ansible$ ls -l
```

#### Output:
```
total 8
-rw-r--r-- 1 thor thor  36 Nov 25 19:20 ansible.cfg
-rw-r--r-- 1 thor thor 237 Nov 25 19:20 inventory
```
## Use Ansible ad-hoc commands to check if any files exist in the destination folder on all app servers.
```
thor@jump_host ~/ansible$ ansible -i inventory all -a "ls -ltrh /opt/dba"
```
#### Output:
```
stapp02 | CHANGED | rc=0 >>
total 0
stapp01 | CHANGED | rc=0 >>
total 0
stapp03 | CHANGED | rc=0 >>
total 0
```
## The playbook.yml file should contain the following contents in order to perform the task on all app servers
```
vi playbook.yml
```

```
- name: Copy text files to Appservers

  hosts: all

  become: yes

  tasks:

    - name: Copy blog.txt to stapp01

      ansible.builtin.copy:

        src: /usr/src/dba/blog.txt

        dest: /opt/dba/

        owner: tony

        group: tony

        mode: "0777"

      when: inventory_hostname == "stapp01"

    - name: Copy story.txt to stapp02

      ansible.builtin.copy:

        src: /usr/src/dba/story.txt

        dest: /opt/dba/

        owner: steve

        group: steve

        mode: "0777"

      when: inventory_hostname == "stapp02"

    - name: Copy media.txt to stapp03

      ansible.builtin.copy:

        src: /usr/src/dba/media.txt

        dest: /opt/dba/

        owner: banner

        group: banner

        mode: "0777"

      when: inventory_hostname == "stapp03"
```      
## To execute the playbook, run the following command 
```
thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml 
```
#### Output:
```
PLAY [Copy text files to Appservers] ********************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [stapp01]
ok: [stapp03]
ok: [stapp02]

TASK [Copy blog.txt to stapp01] *************************************************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Copy story.txt to stapp02] ************************************************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Copy media.txt to stapp03] ************************************************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP **********************************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
```

## Check the files in the destination directory using an ad-hoc command to validate the task
```
thor@jump_host ~/ansible$ ansible -i inventory all -a "ls -ltrh /opt/dba"
```

#### Output:
```
stapp02 | CHANGED | rc=0 >>
total 4.0K
-rwxrwxrwx 1 steve steve 27 Nov 25 19:25 story.txt
stapp01 | CHANGED | rc=0 >>
total 4.0K
-rwxrwxrwx 1 tony tony 35 Nov 25 19:25 blog.txt
stapp03 | CHANGED | rc=0 >>
total 4.0K
-rwxrwxrwx 1 banner banner 22 Nov 25 19:25 media.txt
```
