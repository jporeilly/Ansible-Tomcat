## <font color='red'>Role Info</font>
> Ansible Role to Install Tomcat 9 on CentOS, Fedora, Debian and Ubuntu Linux.

#### <font color='red'>Tested on the following operating systems</font>
- CentOS 8
- CentOS 7
- Fedora 31
- Ubuntu 18.04
- Debian 10

#### <font color='red'> Tasks in the Tomcat Role </font>
This role contains tasks to:
- Install basic packages required
- Install Java
- Add tomcat user and group
- Download tomcat and install - configure systemd
- Configure firewall

#### <font color='red'> Instructions </font>
- Clone the Project:

```
cd ansible-projects/demo/roles
git clone https://github.com/jporeilly/ansible-tomcat.git
```

- Update your inventory, e.g:
```
nano hosts
[tomcat-nodes]
10.0.0.2       # Node IP
```

- Update variables in playbook file - Set Tomcat version, remote user and Tomcat UI access credentials
```
nano tomcat-setup.yml
- name: Tomcat deployment playbook
  hosts: tomcat-nodes       # Inventory hosts group / server to act on
  become: yes               # If to escalate privilege
  become_method: sudo       # Set become method
  remote_user: ansadmin     # Update username for remote server
  vars:
    tomcat_ver: 9.0.30                          # Tomcat version to install
    ui_manager_user: manager                    # User who can access the UI manager section only
    ui_manager_pass: Str0ngManagerP@ssw3rd      # UI manager user password
    ui_admin_username: admin                    # User who can access bpth manager and admin UI sections
    ui_admin_pass: Str0ngAdminP@ssw3rd          # UI admin password
  roles:
    - tomcat
```
If you are using non root remote user, then set username and enable sudo:
```
become: yes
become_method: sudo
```

## Running Playbook
Once all values are updated, you can then run the playbook against your nodes.
Playbook executed as <ansadmin> user - with ssh key:
```
$ ansible-playbook -i hosts tomcat-setup.yml
```
Playbook executed as root user - with password:
```
$ ansible-playbook -i hosts tomcat-setup.yml --ask-pass
```
Playbook executed as sudo user - with password:
```
$ ansible-playbook -i hosts tomcat-setup.yml --ask-pass --ask-become-pass
```
Playbook executed as sudo user - with ssh key and sudo password:
```
$ ansible-playbook -i hosts tomcat-setup.yml --ask-become-pass
```
Playbook executed as sudo user - with ssh key and passwordless sudo:
```
$ ansible-playbook -i hosts tomcat-setup.yml --ask-become-pass
```
Execution should be successful without errors:
