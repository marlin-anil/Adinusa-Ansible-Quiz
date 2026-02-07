Instructions
Note: Execute the quiz intructions in the student user.

Create a new folder named quiz-1 for working directory.
```
mkdir ~/quiz-1

# Must entering
cd ~/quiz-1
```

Create a new file ansible.cfg, define the location of inventory on that file. also, create inventory that stored the pod-username-managed2.
```
# Create ansible.cfg for ansible default confiuration
vim ansible.cfg
[defaults]
inventory=./inventory
host_key_checking=false

# Create inventory to define managed host, replace username with ur real name account
vi inventory
pod-marlin-managed2
```
Create a new playbook named quiz-1_playbook.yml. Add the necessary entries to start a first play named Quiz Playbook. define pod-username-managed2 as target host and student as the remote user ,also add require privilege escalation.
```
vi quiz-1_playbook.yml

---
- name: Quiz Playbook
  hosts: pod-marlin-managed2
  remote_user: student
```

Add the tasks that installs the latest versions of apache2, mariadb-server, php, and php-mysql packages.
```
tasks:
  - name: Installs the latest versions of apache2, mariadb-server, php, and php-mysql packages.
    apt:
     name:
       - apache2
       - mariadb-server
       - php
       - php-mysql
     state: latest
```

Add the tasks to ensure the apache2 and mariadb services are enabled and running.
```
  - name: Ensure the apache2 is enabled and running
    apt:
     name: apache2
     enabled: yes
     state: started
  - name: Ensure the mariadb is enabled and running
    apt:
     name: mariadb
     enabled: yes
     state: started

```

Add the necessary entries to define the final task for generating web content for testing. Use the copy module to copy the text Adinusa quiz Playbook - username on the content parameter to /var/www/html/index.php on pod-username-managed2.
```
# Create jinja2 template, for file content
vi /home/student/quize/index.html
Adinusa quiz Playbook - marlin

# Adding new task on playbook like this following
  - name: Copy content for permord web
    copy:
      src:vi /home/student/quize/index.html
      dest: /var/www/html/index.php

```

Define another play for the task to be performed on the control node. This play will test access to the apache2 web server that should be running on the pod-username-managed2 host. This play does not require privilege escalation, and will run on the localhost.
```
- name : Another play for prepare testing on localhost
  hosts : localhost
  become: false

```

Add a task that tests the web service running on http://pod-username-managed2/index.php from the control node using the uri module. Check for a return status code of 200. After that, save and run the playbook.
```
  tasks:
    - name: Testing web server
      uri:
        url: http://pod-marlin-managed2/index.php
        status_code: 200

```

Check and running playbook for applying 
```
Check sysntax of playbook, to verify no error code before running
ansible-playbook --syntax-check quiz-1_playbook.yml
ansible-playbook quiz-1_playbook.yml

```

For grading our work, running this folloing command
```
nusactl grade anadm-quiz-1
```

  
