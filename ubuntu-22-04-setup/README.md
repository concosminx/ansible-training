# 0. Preparing the servers
a) Install ssh on server **`sudo apt install openssh-server`**

# 1. Preparing your Ansible control node
a) Generate a ssh key with **`ssh-keygen -t ed25519 -C "ansible"`**
b) Test ssh to server **`ssh user@your_remote_server_ip`**
c) Copy the ssh key to server **`ssh-copy-id -i ~/.ssh/ansible.pub SERVER-IP-1`**

# 2 - Create the playbook 
# 3 - Run the playbook
a) **`ansible-playbook playbook.yml -i inventory.ini -l server1 -K`**

	
