### enable|start a service

```yaml
   - name: start and enable httpd (CentOS)
     tags: apache,centos,httpd
     service:
       name: httpd
       state: started
       enabled: yes
     when: ansible_distribution == "CentOS"
```

### modify in a file
```yaml
   - name: change e-mail address for admin
     tags: apache,centos,httpd
     lineinfile:
       path: /etc/httpd/conf/httpd.conf
       regexp: '^ServerAdmin'
       line: ServerAdmin somebody@somewhere.net
     when: ansible_distribution == "CentOS"
     register: httpd
 
   - name: restart httpd (CentOS)
     tags: apache,centos,httpd
     service:
       name: httpd
       state: restarted
     when: httpd.changed 
```



