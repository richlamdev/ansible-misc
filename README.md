# Miscellaneous Ansible playbooks

This is a small collection of playbooks for basic administration and audit of Ubuntu and Redhat based Linux distributions.

Some of the tasks include:

Create/remove users\
Create/remove groups\
Check patching status\
Install patches\
Check firewall/DNS/NTP settings

## Requirements

1) Linux or Mac Operating System.

2) ansible:\
````pip3 install ansible````

3) jinja2 template:\
````pip3 installl jinja2````

4) sshpass:\
````sudo apt install sshpass````\
**Limit use of sshpass for early setup only, due to potential security issues.  Push ssh keys to targets as early as possible**

## Summary of current configuration of ansible configuration (ansible.cfg). Edit as required.
Set inventory file as "inventory" (ini format)\
Set default display of output for better readability\
Set ansible.log in root directory of playbook\
Set default interpreter as python3\
Set some optimizations\
Set host key checking to ignore


## Edit inventory file as required.  Eg:

inventory 

[py3hosts]\
localhost\
192.168.2.3\
my-vmware-cloud\
my-file-server


## Playbook Usage:

````ansible-playbook tasks/adduser.yml -b -k -K````

where:

-b, --become                                  run operations with become (does not imply password prompting)\
-K, --ask-become-pass                         ask for privilege escalation password\
-k, --ask-pass                                ask for connection password

-k, --ask-pass is not required if using SSH keys


******************************************************************************


## Misc Ad-hoc Command examples

### Ping all the assets in inventory file
````ansible all -m ping  -b -k -K````

### Gather facts (information) from assets in inventory file
````ansible all -m setup  -b -k -K````

### Random command execution; alternative to writing a dedicated playbook
````ansible all -a "cat /etc/passwd"  -b -k -K````


where:

-m MODULE_NAME, --module-name MODULE_NAME     module name to execute (default=command)\
-a MODULE_ARGS, --args MODULE_ARGS            module arguments\
-b, --become                                  run operations with become (does not imply password prompting)\
-K, --ask-become-pass                         ask for privilege escalation password\
-k, --ask-pass                                ask for connection password

-k, --ask-pass is not required if using SSH keys
