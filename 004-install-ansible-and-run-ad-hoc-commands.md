### Install ansible
- `sudo apt update` - update the index
- `sudo apt install ansible` - install ansible from ubuntu default repos

### Create an inventory file
- cd into the git repo
- `nano inventory` and type the server addresses (u can also use DNS)
```
SERVER_IP_1
SERVER_IP_2
SERVER_IP_3
```

### Run ad-hoc command
- `ansible all --key-file ~/.ssh/ansible -i inventory -m ping` - run ping on all inventory servers
- `ansible all --list-hosts` - list all the hosts
- `ansible all -m gather_facts --limit SERVER_IP_1` - info about particular server
-  

### Create ansible config file
- `nano ansible.cfg`
```
[defaults]
inventory = inventory
private_key_file = ~/.ssh/ansible
```
- `ansible all -m ping` (with the config file)

### Default ansible file
- `ls /etc/ansible`
