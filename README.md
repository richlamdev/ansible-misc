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
````pip3 install jinja2````

4) sshpass:\
````sudo apt install sshpass````\
** *Limit use of sshpass for early setup only, due to potential security issues.  Deploy ssh keys to target hosts as early as possible.* **

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

-b, --become                   run operations with become (does not imply password prompting)\
-K, --ask-become-pass          ask for privilege escalation password\
-k, --ask-pass                 ask for connection password

-k, --ask-pass is not required if using SSH keys


## adduser-ansible-sudo.yml

This playbook creates a sudo user on newly provisioned VM(s) with SSH key deployed.
Requires the play executed from the context of the new user on from the ansible "push" server (host machine).\

Example:

Create the user "ansible-user" as a sudo user to all the newly provisioned VM's.  All commands to be executed on the host or local machine.

1) Create ansible-user:\
````sudo adduser ansible-user````

2) Switch or log on as the ansible-user:\
````su - ansible-user````

3) Generate an SSH keypair\
````ssh-keygen -b 4096 -t rsa````

4) Edit inventory file to include the VM's to have this user deployed to.

5) From the roote of the git repo, execute the play to create the ansible-user on the new VM's:\
````ansible-playbook tasks/initial-ansible-user.yml -bkK -u <root user or sudo user>````

6) Test the user was created with an SSH key on the new VM(s):\
````ssh ansible-user@<IP-of-new-vm>````


## Roles

Brief note, if the ansible-role-nginx-tls is used, be aware that
deployment of TLS private key certificate should _not_ be provisioned this way.
This is implemented here as a quick and simple example of how it could be done.
Primarily for the purposes of being to able to view/test TLS certificates via
gui, instead via openssl.

For test/development/temporary TLS certificate creation via HashiCorp Vault,
see this repo:
[HashiCorp Vault TLS Certificates with raft backend](https://github.com/richlamdev/vault-pki-raft)


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
