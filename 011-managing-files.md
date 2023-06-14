### copy a file 

```yaml
   - name: copy html file for site
     tags: apache,apache,apache2,httpd
     copy:
       src: default_site.html
       dest: /var/www/html/index.html
       owner: root
       group: root
       mode: 0644
```

### run the playbook
- ` ansible-playbook --ask-become-pass file_management.yml`

### file_management.yml (updated)
```yaml
- hosts: workstations
   become: true
   tasks:

   - name: install unzip
     package:
       name: unzip
 
   - name: install terraform
     unarchive:
      src: https://releases.hashicorp.com/terraform/0.12.28/terraform_0.12.28_linux_amd64.zip
       dest: /usr/local/bin
       remote_src: yes
       mode: 0755
       owner: root
       group: root
```