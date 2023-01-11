## Go through the task's folder 

```
thor@jump_host ~$ cd /home/thor/ansible/
```
```
thor@jump_host ~/ansible$ ls -l
```
```
total 8
-rw-r--r-- 1 thor thor  36 Jan  5 10:44 ansible.cfg
-rw-r--r-- 1 thor thor 237 Jan  5 10:44 inventory
```


## Add the following lines to a playbook, then save and exit.

```
thor@jump_host ~/ansible$ vi playbook.yml
```
```
---
- name: Ansible replace

  hosts: stapp01,stapp02,stapp03

  become: yes

  tasks:

    - name: blog.txt replacement

      replace:

        path: /opt/data/blog.txt

        regexp: "xFusionCorp"

        replace: "Nautilus"

      when: inventory_hostname == "stapp01"

    - name: story.txt replacement

      replace:

        path: /opt/data/story.txt

        regexp: "Nautilus"

        replace: "KodeKloud"

      when: inventory_hostname == "stapp02"

    - name: media.txt replacement

      replace:

        path: /opt/data/media.txt

        regexp: "KodeKloud"

        replace: "xFusionCorp Industries"

      when: inventory_hostname == "stapp03"
```
      
## To execute the playbook, run the following command

```      
thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml
```
#### Output
```
PLAY [Ansible replace] **********************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [stapp01]
ok: [stapp03]
ok: [stapp02]

TASK [blog.txt replacement] *****************************************************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [story.txt replacement] ****************************************************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [media.txt replacement] ****************************************************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP **********************************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0 
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0 
      
```
