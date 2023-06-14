### Targeting specific nodes

### install_apache.yml
```yaml
 ---
 
 - hosts: all
   become: true
   tasks:
 
   - name: install apache and php for Ubuntu servers
     apt:
       name:
         - apache2
         - libapache2-mod-php
       state: latest
       update_cache: yes
     when: ansible_distribution == "Ubuntu"
 
   - name: install apache and php for CentOS servers
     dnf:
       name:
         - httpd
         - php
       state: latest
       update_cache: yes
     when: ansible_distribution == "CentOS"
```

### inventory (updated with groups)
```bash
[web_servers]
SERVER_IP_1
SERVER_IP_2
 
[db_servers]
SERVER_IP_3

[file_servers]
SERVER_IP_4
```

### site.yml
```yaml
 ---
 
 - hosts: all
   become: true
   tasks:
 
   - name: install updates (CentOS)
     dnf:
       update_only: yes
       update_cache: yes
     when: ansible_distribution == "CentOS"
 
   - name: install updates (Ubuntu)
     apt:
       upgrade: dist
       update_cache: yes
     when: ansible_distribution == "Ubuntu"
 
 
 - hosts: web_servers
   become: true
   tasks:
 
   - name: install apache and php for Ubuntu servers
     apt:
       name:
         - apache2
         - libapache2-mod-php
       state: latest
     when: ansible_distribution == "Ubuntu"
 
   - name: install apache and php for CentOS servers
     dnf:
       name:
         - httpd
         - php
       state: latest
     when: ansible_distribution == "CentOS"
```

### site.yml (second version)
```yaml
 ---
 
 - hosts: all
   become: true
   pre_tasks:
 
   - name: install updates (CentOS)
     dnf:
       update_only: yes
       update_cache: yes
     when: ansible_distribution == "CentOS"
 
   - name: install updates (Ubuntu)
     apt:
       upgrade: dist
       update_cache: yes
     when: ansible_distribution == "Ubuntu"
 
 
 - hosts: web_servers
   become: true
   tasks:
 
   - name: install apache2 package
     apt:
       name:
         - apache2
         - libapache2-mod-php
       state: latest
     when: ansible_distribution == "Ubuntu"
 
   - name: install httpd package
     dnf:
       name:
         - httpd
         - php
       state: latest
     when: ansible_distribution == "CentOS"
```

### site.yml (third version)
```yaml
---
 
 - hosts: all
   become: true
   pre_tasks:
 
   - name: install updates (CentOS)
     dnf:
       update_only: yes
       update_cache: yes
     when: ansible_distribution == "CentOS"
 
   - name: install updates (Ubuntu)
     apt:
       upgrade: dist
       update_cache: yes
    when: ansible_distribution == "Ubuntu"
 
 
 - hosts: web_servers
   become: true
   tasks:
 
   - name: install httpd package (CentOS)
     dnf:
       name:
         - httpd
         - php
       state: latest
     when: ansible_distribution == "CentOS"
 
   - name: install apache2 package (Ubuntu)
     apt:
       name:
         - apache2
         - libapache2-mod-php
       state: latest
     when: ansible_distribution == "Ubuntu"
 
 - hosts: db_servers
   become: true
   tasks:
 
   - name: install httpd package (CentOS)
     dnf:
       name: mariadb
       state: latest
     when: ansible_distribution == "CentOS"
 
   - name: install mariadb server
     apt:
       name: mariadb-server
       state: latest
     when: ansible_distribution == "Ubuntu"
```


### site.yml (third version)
```yaml
 ---
 
 - hosts: all
   become: true
   pre_tasks:
 
   - name: install updates (CentOS)
     dnf:
       update_only: yes
       update_cache: yes
     when: ansible_distribution == "CentOS"
 
   - name: install updates (Ubuntu)
     apt:
       upgrade: dist
       update_cache: yes
     when: ansible_distribution == "Ubuntu"
 
 
 - hosts: web_servers
   become: true
   tasks:
 
   - name: install httpd package (CentOS)
     dnf:
       name:
         - httpd
         - php
       state: latest
     when: ansible_distribution == "CentOS"
 
   - name: install apache2 package (Ubuntu)
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
     dnf:
       name: mariadb
       state: latest
     when: ansible_distribution == "CentOS"
 
   - name: install mariadb server
     apt:
       name: mariadb-server
       state: latest
     when: ansible_distribution == "Ubuntu"
 
 - hosts: file_servers
   become: true
   tasks:
 
   - name: install samba package
     package:
       name: samba
       state: latest
```