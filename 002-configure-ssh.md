### Install the OpenSSH server on ubuntu 
- `sudo apt install openssh-server`

### Make the initial connection via ssh
- `ssh SERVER_1_IP`

### Create an ssh key pair (with a passphrase) for the normal user
- `ip a` - get the ip address
- `ssh server1` - check the ssh from the ansible host
- `Ctrl + D` - log out
- `ls -la .ssh` - see the content of ssh folder
- **`ssh-keygen -t ed25519 -C "default key"`**
- provide a location and passphrase
- `ls -la .ssh` - see the generated files
- `cat .ssh/id_ed25519.pub` - see the content of the public key
- **`ssh-copy-id -i ~/.ssh/id_ed25519.pub SERVER_1_IP`**
- `cat .ssh/authorized_keys` - check the new authorized key in SERVER_1_IP

### Create a ssh key pair for ansible
- the same process [another comment, a new path to save, no passphrase]

### Tell ssh to use a specific key for the connection
- `ssh -i ~/.ssh/ansible SERVER_1_IP`

### Use the ssh-agent to cache the passphrase
- `eval $(ssh-agent)`
- `ps aux | grep PID`
- `ssh-add` - add the identity to ssh-agent
- `alias ssha='eval $(ssh-agent) && ssh-add'` - create a convenient alias 
- `nano .bashrc` - add the alias between restarts
- `alias ssha` - check the `ssha` alias
