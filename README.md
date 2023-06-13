# ansible-training
Ansible Training

# 014-roles.md

### create a roles directory

- `mkdir roles`

### create a directory for each role you wish to add:

```shell
cd roles
 mkdir base
 mkdir db_servers
 mkdir file_servers
 mkdir web_servers
 mkdir workstations
```

### inside each role directory, create a tasks directory

```shell
cd <role_name>
 mkdir tasks
```

### site.yml (new version)

```yaml
---
 
 - hosts: all
   become: true
   pre_tasks:
 
   - name: update repository index (CentOS)
     tags: always
     dnf:
       update_cache: yes
     changed_when: false
     when: ansible_distribution == "CentOS"
 
   - name: update repository index (Ubuntu)
     tags: always
     apt:
       update_cache: yes
     changed_when: false
     when: ansible_distribution == "Ubuntu"
 
 - hosts: all
   become: true
   roles:
     - base
    
 - hosts: workstations
   become: true
   roles:
     - workstations
 
 - hosts: web_servers
   become: true
   roles:
     - web_servers
 
 - hosts: db_servers
   become: true
   roles:
     - db_servers
 
 - hosts: file_servers
   become: true
   roles:
     - file_servers
```

### main.yml (db_servers role)

```yaml
 - name: install mariadb server package (CentOS)
   tags: centos,db,mariadb
   dnf:
     name: mariadb
     state: latest
   when: ansible_distribution == "CentOS"
 
 - name: install mariadb server
   tags: db,mariadb,ubuntu
   apt:
     name: mariadb-server
     state: latest
   when: ansible_distribution == "Ubuntu"
```

### 015-host-variables-and-handlers

host_vars (ubuntu web server)
 apache_package_name: apache2
 apache_service: apache2
 php_package_name: libapache2-mod-php
host_vars (ubuntu web server)
 apache_package_name: httpd
 apache_service: httpd
 php_package_name: php
main.yml (web_servers role)
 - name: install web server packages
   tags: apache,apache2,centos,httpd,ubuntu
   package:
     name:
       - "Template:Apache package name"
       - "Template:Php package name"
     state: latest
   when: ansible_distribution == "CentOS"
 
 - name: start and enable apache
   tags: apache,centos,httpd
   service:
     name: "Template:Apache service"
     state: started
     enabled: yes
 
 - name: change e-mail address for admin
   tags: apache,centos,httpd
   lineinfile:
     path: /etc/httpd/conf/httpd.conf
     regexp: '^ServerAdmin'
     line: ServerAdmin somebody@somewhere.net
   when: ansible_distribution == "CentOS"
   notify: restart_apache
 
 - name: copy html file for site
   tags: apache,apache,apache2,httpd
   copy:
     src: default_site.html
     dest: /var/www/html/index.html
     owner: root
     group: root
     mode: 0644
Handlers for the web_servers role
Create the handlers directory (within the role directory):

 mkdir handlers
 - name: restart_apache
   tags: apache,centos,httpd
   service:
     name: "Template:Apache service"
     state: restarted

# 016-templates

 ansible-playbook site.yml
Adding a default value to the variable
In the template, change the “AllowUsers” line to:

 AllowUsers Template:Ssh users
Taking it a bit further, add another variable to the template to determine whether or not password authentication is allowed for a host:

....