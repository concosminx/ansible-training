### location of OpenSSH daemon config file (used as an example for the template)
- `/etc/ssh/sshd_config`

### in the role directory
- create the directory for templates: `mkdir templates`
- copy the sshd config file from a server to this directory with the *.j2 extension: `sshd_config_ubuntu.j2`


### adding a variable inside the OpenSSH config file template

- inside the template(s), add:
```
 AllowUsers {{ ssh_users }}
```
- note: Make sure the “AllowUsers” option isn’t already in the file, if so, replace it with the above)

### adding the variable to a host
- inside the host_vars directory, we should already have a hosts variable file for each server. - add the new variable inside the file(s):
```
 ssh_users: "jay simone"
 ssh_template_file: sshd_config_ubuntu.j2
```
- Note: Make sure you change the usernames in the ssh_users variable to be the actual usernames you’re using for yourself as well as the ansible user.

### having Ansible render the template

- inside the base role, in the main.yml file, add this play:
```yaml
 - name: openssh | generate sshd_config file from template
   tags: ssh
   template:
     src: "Template:Ssh template file"
     dest: /etc/ssh/sshd_config
     owner: root
     group: root
     mode: 0644
   notify: restart_sshd
```

### create a handler to restart the OpenSSH daemon
- inside the base role, create a directory for handlers: `mkdir handlers`
- go inside that directory, and create a ‘main.yml’ file
```yaml
 - name: restart_sshd
   service:
     name: sshd
     state: restarted
```

### run the playbook
- `ansible-playbook site.yml`