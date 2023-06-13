### Elevated ad-hoc commands

- `ansible all -m apt -a update_cahe=true` - needs elevated rights
- `ansible all -m apt -a update_cahe=true --become --ask-become-pass` - tell ansible to use sudo (become)
- `ansible all -m apt -a name=vim-nox --become --ask-become-pass` - install a package via the apt module
- `apt search vom-nox` - check if the package is installed
- `ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass` - install a package via the apt module, and also make sure it’s the latest version available
- `ansible all -m apt -a upgrade=dist --become --ask-become-pass` - upgrade all the package updates that are available
