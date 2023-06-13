### The **when** Conditional

```yaml
---
 
- hosts: all
  become: true
  tasks:
 
  - name: update repository index
    apt:
      update_cache: yes
    when: ansible_distribution == "Ubuntu"
 
  - name: install apache2 package
    apt:
      name: apache2
      state: latest
    when: ansible_distribution == "Ubuntu"
 
  - name: add php support for apache
    apt:
      name: libapache2-mod-php
      state: latest
    when: ansible_distribution == "Ubuntu"
 
  - name: update repository index
    dnf:
      update_cache: yes
    when: ansible_distribution == "CentOS"
 
  - name: install httpd package
    dnf:
      name: httpd
      state: latest
    when: ansible_distribution == "CentOS"

  - name: add php support for apache
    dnf:
      name: php
      state: latest
    when: ansible_distribution == "CentOS"
```

### install_apache.yml (condensed)
```yaml
---
 
- hosts: all
  become: true
  tasks:
 
  - name: update repository index
    apt:
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

  - name: install apache2 and php packages for Ubuntu
    apt:
      name:
        - apache2
        - libapache2-mod-php
      state: latest
    when: ansible_distribution == "Ubuntu"

  - name: update repository index
    dnf:
      update_cache: yes
    when: ansible_distribution == "CentOS"

  - name: install apache and php packages for CentOS
    dnf:
      name:
        - httpd
        - php
      state: latest
    when: ansible_distribution == "CentOS"
```

### install_apache.yml (further condensed)
```yaml
---
 
- hosts: all
  become: true
  tasks:
 
  - name: install apache2 package
    apt:
      name:
        - apache2
        - libapache2-mod-php
      state: latest
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

  - name: install httpd package
    dnf:
      name:
        - httpd
        - php
      state: latest
      update_cache: yes
    when: ansible_distribution == "CentOS"
```

### install_apache.yml (condensed even futher)
```yaml
---
 
- hosts: all
  become: true
  tasks:
 
  - name: install apache and php
    package:
      name:
        - "{{ apache_package }}"
        - "{{ php_package }}"
      state: latest
      update_cache: yes
```

### inventory file (with host variables added)
```shell
SERVER_1_IP apache_package=apache2 php_package=libapache2-mod-php
SERVER_2_IP apache_package=apache2 php_package=libapache2-mod-php
SERVER_3_IP apache_package=apache2 php_package=libapache2-mod-php
SERVER_4_IP apache_package=httpd php_package=php
```