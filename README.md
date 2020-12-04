# Miscellaneous Ansible playbooks

## Summary of current configuration of ansible configuration (ansible.cfg). Edit as required.
Set inventory file as "inventory" (ini format)\
Set default display of output for better readability\
Set ansible.log in root directory of playbook\
Set default interpreter as python3\
Set some optimizations\
Set host key checking to ignore\


## Edit inventory file as required.  Eg:

inventory
[py3hosts]\
localhost\
192.168.2.3\
my-vmware-cloud\
my-file-server\


## Playbook Usage:

````ansible-playbook tasks/adduser.yml -b -k -K````

where:

-b, --become             run operations with become (does not imply password prompting)
-K, --ask-become-pass    ask for privilege escalation password
-k, --ask-pass           ask for connection password

-k, --ask-pass is not required if using SSH keys


******************************************************************************


## Misc Ad-hoc Command examples

### ping all the assets in inventory file
````ansible all -m ping  -b -k -K````

### gather facts (information) from assets in inventory file
````ansible all -m setup  -b -k -K````

### random command execution; alternative to writing a dedicated playbook
````ansible all -a "cat /etc/passwd"  -b -k -K````


where:

-b, --become             run operations with become (does not imply password prompting)
-K, --ask-become-pass    ask for privilege escalation password
-k, --ask-pass           ask for connection password

-k, --ask-pass is not required if using SSH keys
