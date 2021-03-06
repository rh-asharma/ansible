# Ansible Playbooks

Some example playbooks which can be used with ansible core or via tower

### Setup

git clone this repo: 
```
cd 
git clone https://github.com/ffirg/ansible.git
cd ansible
```

#### Facts
facts.all contains all the facts as discovered by the gather_facts module for a typical Linux host.

#### directory-structure.txt
Describes the structure of a role based playbook.

#### Inventory hosts file

I use a local hosts file to showcase how you can logically split hosts into [geographical] groups, and then from parent groups from other groups. I also set some group variables at this level - used in the post-provision.yml for example.

#### first-checks.yml 
```
ansible-playbook -i hosts first-checks.yml
```

For a particular host, try it ping it, get the default IP gateway if set, set the hostname, configure and restart firewalld

Shows: ping, hostname firewall modules, tagging, event notify handler

#### extras.yml 
```
ansible-playbook -i hosts extras.yml
```

For all hosts apart from [cloud] ones, install the wget package
Shows how to use the hosts: directive to be more specific against the playbook.

#### blocks-eg.yml 
```
ansible-playbook -i hosts blocks-eg.yml
```

An example of how to use the Ansible 2.x blocks feature.
Sets conditions around a code block (distribution= RedHat) and how to do generic error handling within it (rescue/always)

Shows: ping, command, local_action modules, blocks with error handling

#### post-provision.yml 
```
ansible-playbook -i hosts post-provision.yml
```

For one set of hosts, install wget and add some users. Run a local command (say on Mac OS X) after using vars from hosts file
For another set of hosts, update MOTD and add some users. Run a local command (say on Mac OS X) after using vars from hosts file
Note: MOTD will fail. This is deliberate to show how Ansible handles failures (colour coding etc).
Re-run the playbook after creating /tmp/foo to fix the problem.

Shows: yum, user account, copy, local_action modules, tagging

To show how tagging works:
```
ansible-playbook -i hosts post-provision.yml --tags "users, packages"
ansible-playbook -i hosts post-provision.yml --skip-tags "notify"
```

#### reboot.yml 
```
ansible-playbook -i hosts reboot.yml
```

CAUTION!! This will probably always reboot a host so make sure you want to do that!
Forces a reboot, based on kernel revision mismatch, and waits for SSH port 22 to be up again

Shows: shell, command, local_action modules. Storing a variable and acting upon it (register/reboot)

#### deploy-vm-amazon.yml 
```
ansible-playbook -i hosts deploy-vm-amazon.yml 
```

Creates a VM instance in AWS EC2. You will need to change to reflect your needs and own account.

Shows: How to run from your local client, ec2, wait_for, add_host modules. Delegation (to localhost) and waiting for port readiness.

#### amazon.yml 
```
ansible-playbook -i hosts amazon.yml 
```
After creating a new VM instance, you can run this playbook to install a web server and add some user accounts.

Shows: yum, user account modules, for loop and service handler restart

#### deploy-vm-azure.yml 
```
ansible-playbook -i hosts deploy-vm-azure.yml 
```

Creates a VM instance in Microsoft Azure Cloud. You will need to change to reflect your needs and own account.

Shows: How to run from your local client, azure, wait_for, add_host modules. Delegation (to localhost) and waiting for port readiness.
NOTE: currently broken. There is a missing 'instance' setting in the module causing a failure :(

#### azure.yml 
```
ansible-playbook -i hosts azure.yml 
```
After creating a new classic VM, you can run this playbook to install a web server and NTP and add some user accounts.
Calls the correct package manager installer and packages based on OS Distribution.

Shows: yum, apt, user account modules, for loop based on Ansible discovered fact conditions (OS distribution)

#### deploy-vm-vsphere-from-template.yml
```
ansible-playbook deploy-vm-vsphere-from-template.yml
```
Example of how easy it is to create a new vsphere VM within vcenter by cloning from an existing VM template. (Needs VPN and account login to vcenter)

#### delete-vm-vsphere.yml
```
ansible-playbook delete-vm-vsphere.yml
```
Example of how to delete a vpshere VM within vcenter. (Needs VPN and account login to vcenter)

### More Cloud Examples - see https://github.com/ffirg/ansible/blob/master/multi_cloud
### Windows examples - see https://github.com/ffirg/ansible/blob/master/windows
