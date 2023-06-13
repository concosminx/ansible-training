### create a new user

```yaml
 - hosts: all
   become: true
   tasks:

   - name: create simone user
     tags: always
     user:
       name: simone
       groups: root
```

### sudoer_simone
```txt
simone ALL=(ALL) NOPASSWD: ALL
``` 

### copies sudoer file
```yaml
   - name: add sudoers file for simone
     tags: always
     copy:
       src: sudoer_simone
       dest: /etc/sudoers.d/simone
       owner: root
       group: root
       mode: 0440
```

### bootstrap.yml
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
 
 - hosts: all
   become: true
   tasks:
 
   - name: create simone user
     user:
       name: simone
       groups: root
 
   - name: add ssh key for simone
     authorized_key:
       user: simone
       key: "ssh-ed25519 AAAAC... ansible"
      
   - name: add sudoers file for simone
     copy:
       src: sudoer_simone
       dest: /etc/sudoers.d/simone
       owner: root
       group: root
       mode: 0440
```

### Use `changed_when: false` to 