# 1 - Preparing your Ansible control node
    - create the inventory file 
	- generate a ssh key `ssh-keygen -t ed25519 -C "ansible"`
	- install ssh on server `sudo apt install openssh-server` 
	- test ssh to server `ssh root@your_remote_server_ip` or `ssh your_remote_server_ip`
	
# 2 - Create the playbook 
	- see the files 

# 3 - Run the playbook
	- `ansible-playbook playbook.yml -i inventory.ini -l server1 -u root -k` (with password)
	- `ansible-playbook playbook.yml -i inventory.ini -l server1 -K` (with ssh key)

	