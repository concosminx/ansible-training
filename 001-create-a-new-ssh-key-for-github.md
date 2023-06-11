#Generate a new SSH key
- `ssh-keygen -t ed25519 -C "your_email@example.com"` or
- `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"` (for legacy systems)


#Add the key to the SSH agent (Ubuntu)
- `eval "$(ssh-agent -s)"`
- `ssh-add ~/.ssh/id_ed25519`

#Add the public key to Github
- `cat ~/.ssh/id_ed25519.pub`
