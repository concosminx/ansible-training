# ansible-training
Ansible Training

---

Source [https://www.youtube.com/@LearnLinuxTV](https://www.youtube.com/@LearnLinuxTV)

---

## Testing Connectivity to Nodes
- `ansible all -m ping`

## Connecting as a Different User
- `ansible all -m ping -u sammy`
- `ansible-playbook myplaybook.yml -u sammy`

## Using a Custom SSH Key
- `ansible all -m ping --private-key=~/.ssh/custom_id`
- `ansible-playbook myplaybook.yml --private-key=~/.ssh/custom_id`

## Using Password-Based Authentication
- `ansible all -m ping --ask-pass`
- `ansible-playbook myplaybook.yml --ask-pass`

## Providing the `sudo` Password
- `ansible all -m ping --ask-become-pass`
- `ansible-playbook myplaybook.yml --ask-become-pass`

## Using a Custom Inventory File (default inventory file is typically located at /etc/ansible/hosts)
- `ansible all -m ping -i my_custom_inventory`
- `default inventory file is typically located at /etc/ansible/hosts`

## Running ad-hoc Commands
 - `ansible all -a "uname -a"`
 - `ansible server1 -m apt -a "name=vim"`
 - `ansible server1 -m apt -a "name=vim" --check` - dry run

## Running Playbooks
- `ansible-playbook myplaybook.yml`
- `ansible-playbook -l server1 myplaybook.yml`

## Getting Information about a Play
- `ansible-playbook myplaybook.yml --list-tasks`
- `ansible-playbook myplaybook.yml --list-hosts`
- `ansible-playbook myplaybook.yml --list-tags`

## Controlling Playbook Execution
- `ansible-playbook myplaybook.yml --start-at-task="Set Up Nginx"`
- `ansible-playbook myplaybook.yml --tags=mysql,nginx`
- `ansible-playbook myplaybook.yml --skip-tags=mysql`

## Using Ansible Vault to Store Sensitive Data
### Creating a New Encrypted File
- `ansible-vault create credentials.yml`
### Encrypting an Existing Ansible File
- `ansible-vault encrypt credentials.yml`
### Viewing the Contents of an Encrypted File
- `ansible-vault view credentials.yml`
### Editing an Encrypted File
- `ansible-vault edit credentials.yml`
### Decrypting Encrypted Files
- `ansible-vault decrypt credentials.yml`


## Using Multiple Vault Passwords
- `ansible-vault create --vault-id dev@prompt credentials_dev.yml`
- `ansible-vault create --vault-id prod@prompt credentials_prod.yml`
- `ansible-vault edit credentials_dev.yml --vault-id dev@prompt`

## Running a Playbook with Data Encrypted via Ansible Vault
- `ansible-playbook myplaybook.yml --ask-vault-pass`
- `ansible-playbook myplaybook.yml --vault-id dev@prompt`

## Debugging
- `ansible-playbook myplaybook.yml -v`
- `ansible-playbook myplaybook.yml -vvvv`