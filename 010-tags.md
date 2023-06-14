### site.yml (with tags added)
```yaml
 ---
 
 - hosts: all
   become: true
   pre_tasks:
 
   - name: install updates (CentOS)
     tags: always
     dnf:
       update_only: yes
       update_cache: yes
     when: ansible_distribution == "CentOS"
 
   - name: install updates (Ubuntu)
     tags: always
     apt:
       upgrade: dist
       update_cache: yes
     when: ansible_distribution == "Ubuntu"
 
 
 - hosts: web_servers
   become: true
   tasks:
 
   - name: install httpd package (CentOS)
     tags: apache,centos,httpd
     dnf:
       name:
         - httpd
         - php
       state: latest
     when: ansible_distribution == "CentOS"
 
   - name: install apache2 package (Ubuntu)
     tags: apache,apache2,ubuntu
     apt:
       name:
         - apache2
         - libapache2-mod-php
       state: latest
     when: ansible_distribution == "Ubuntu"
 
 - hosts: db_servers
   become: true
   tasks:
 
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
 
 - hosts: file_servers
   tags: samba
   become: true
   tasks:
 
   - name: install samba package
     tags: samba
     package:
       name: samba
       state: latest
```

### list the available tags in a playbook
- `ansible-playbook --list-tags site_with_tags.yml`


### examples of running a playbook but targeting specific tags
- `ansible-playbook --tags db --ask-become-pass site_with_tags.yml`
- `ansible-playbook --tags centos --ask-become-pass site_with_tags.yml`
- `ansible-playbook --tags apache --ask-become-pass site_with_tags.yml`