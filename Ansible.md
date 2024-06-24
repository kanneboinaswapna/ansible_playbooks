# Ansible intro:
It is a open-source automation configuration management tool,deploy applications,that can reduces the complexicity.

By using ansible you can automate any tasks on virtually.It uses human readable scripts called playbooks to automate the tasks.
Here are some common usecases

eliminates the repitation process and simplifies the works

manage and maintain system configuration

continously deploy complex applications

performs zero downtime applications rolling updates.

## some principles/ features

### Agentless architecture
avoid additional installation of softwares
### simplicity
The playbooks are YAML format easy to understand.
### Scalability and flexibility
It can easily scales the systems through larger range of OS,Colud platforms, and network design
### Idempotence:
If you have larger playbook, it doesnot make any un-neccessary changes even you can playbook multiple times.

# SETUP
Control node: where ansible install
Managed nodes: where all excutions are works, these are target devices
```
pip install ansible
2 machines(ansible node,worker node)
sudo apt update && sudo apt install Python 3
install ansible on control node-- Sudo apt install ansible -y
1.passwordless authentication-- sudo vi /etc/ssh/sshd_config & sudo system restart sshd
2.Add user name and passwords on bothnodes
3.add sudo user --sudo visudo
4.communication by SSH (ssh keygen, ~/.ssh/id_rsa)
5.in ansible node {ssh 2nd node ip}
6.check in node cat ~/.ssh/authorized keys
7.ssh-copy-id username@IP
create folder---mkdir ansible, cd ansible
make inventory file--inventory.yml or inventory.ini
make playbook.yaml
```

## Building inventory
inventory is a list of managed nodes,that have information about each node like IP,DNS.Sometimes an inventory source file is also referred to as a ‘hostfile’.
```
[hosts]
192.0.2.50
192.0.2.51
192.0.2.52
```
check-- ansible -m ping -i <inventoryfilename> all

```
myhosts:
  hosts:
    my_host_01:
      ansible_host: 192.0.2.50
    my_host_02:
      ansible_host: 192.0.2.51
    my_host_03:
      ansible_host: 192.0.2.52
```
```
---
locals
  hosts: 
    x.x.x.x:
nodes:
  hosts:
   X.x.x.x:
   x.x.x.x:
ubuntu:
  hosts:
    x.x.x.x:
    x.x.x.x:
redhat:
  hosts:
    x.x.x.x:
    x.x.x.x:
```

# Creating playbook
Playbook is a Yaml file, that can be series of tasks that are excute on remote systems.
It is order of operations top to bottom overall a goal


## Module
modules are nothing but a commands , these are building blocks of ansible tasks.They are small programmes that are performed on worker nodes, such as installing, updating, package, copying files...

apt module is used to install a package
```
- name: Install Nginx
   ansible.builtin.apt:
    name: nginx
    state: present
```
### TASKS
Tasks are individual actions within a play that use modules to perform operations on worker nodes. Each task is excuted in order and include conditionals, loops, handlers.
```
- name: Install Nginx
  ansible.builtin.apt:
    name: nginx
    state: present

- name: Start Nginx service
  service:
    name: nginx
    state: started
```

## Example playbbok
```
---
 - name: activity1
   become: yes
   hosts: all
   tasks:
     - name: apache2 installation
       ansible.builtin.apt:
         name: apache2
         state: present
         update_cache: yes
```
```
---
 - name: activity2
   become: yes
   hosts: all
   tasks:
     - name: php installation on apache2
       ansible.builtin.apt:
         name: 
           - apache2
           - php 
           - libapache2-mod-php 
           - php-mysql
         state: present
         update_cache: yes
     - name: copy php file
       ansible.builtin.copy:
         content: '<?php phpinfo(); ?>'
         dest: /var/www/html/info.php
         remote_src: yes
```
# Roles
Collection of playbooks, that are reusable and shared tasks,these having specific directories includes with tasks,modules,playbooks,handlers...

# Handlers:
handlers are specific tasks that are notified by other tasks, generally used for restart the services and perform whenever change has occured.
```
handlers:
 -name:my-task
   service:
     name:my-service
     start: restarted
```
## Plugins
It is a piece of a code that expands ansible capabilities and makes high performace.

## Collections
That having ansible content is distributed that contains playbooks,roles,modules,and plugins.

Collection can intall through ansible galaxy.
## Ansible Galaxy


   
