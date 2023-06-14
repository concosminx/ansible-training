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