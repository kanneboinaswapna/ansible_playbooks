# Ansible intro:
Ansible is python developed tool.
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
Collection of playbooks, that are reusable and shared tasks,these having specific directories includes with tasks,modules,playbooks,handlers...,It is manages complex playbooks easily.

Roles allow you to break down your playbooks into reusable components

Default roles directory: /etc/ansible/roles

```
my_role/
├── defaults
│   └── main.yml
├── files
│   └── example.conf
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
│   └── example.j2
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
### defaults: 
contains default variables for the role.
### files:
having static files that can be deployed by the role.
### handlers:
contains handlers, which are tasks triggered by the other tasks
### meta
contains metadata about role such as author,licence...
### tasks:
Contains the main list of tasks to be executed by the role.
### templates:
Contains Jinja2 templates that can be used to dynamically generate configuration files.
### vars:
Contains variables that are more static and should override defaults.

## creating role

```
ansible-galaxy init my_role {my_role -- role name}
## using role in playbook
---
- name: Configure Web Servers
  hosts: webservers
  roles:
    - my_role
```

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

Default collection directory: ~/.ansible/collections

using collections
```
- hosts: all
  tasks:
    - name: Ensure latest version of Apache is installed
      community.general.package:
        name: httpd
        state: latest
```

# Ansible Galaxy
It is community focused site for sharing roles and collections.it is essential part of ansible eco-system.

It is a community repository that have roles and collections.

Roles are organizes the playbooks where as collections are have roles,modules,plugins,documentation...

### working of galaxy

ansible-galaxy install <role_name>
ansible-galaxy collection  install <collection_name>
```
- hosts: webservers
  roles:
    - geerlingguy.apache
```
using collections
```
- hosts: all
  tasks:
    - name: Ensure latest version of Apache is installed
      community.general.package:
        name: httpd
        state: latest
```

You can publish roles and collections to ansible-galaxy through github integration
```ansible-galaxy role import <GitHub-username>/<repository-name>```

```ansible-galaxy collection publish <path-to-collection-tarball>```

## Uses of ansible galaxy
Time saving,Best practices,community collabration

# Ansible UI
Ansible UI it is oftenly referred as AWX/ Redhat ansible tower,It is a web based user interface for ansible.

These tools enhance the capabilities of Ansible by adding a graphical interface, centralized management, scheduling, RBAC, and other enterprise features, making it easier to deploy, manage, and monitor automation tasks across your infrastructure.

## AWX
It is a open source upstream project of redhat Ansible tower. It offers web based UI, REST API...

### Features of AWX
1.Dashboard: provides a status of recent job activity of system.

2.Inventory management: Allows to manage hosts and groups

3.Job template: Define playbooks, credentials, and other parameters for running Ansible jobs.

4.Schedules: Schedule jobs to run at specific times.

5.Credentials Management: Securely store and manage credentials for different systems.

6.RBAC (Role-Based Access Control): Control who can see and do what within the AWX environment.

7.Manage your playbook source code, which can be pulled from various SCMs (Source Control Management systems) like Git.

## Redhat ansible tower:

Ansible tower is a further version of AWX. It includes additional features like scalability, enhancements

Enhanced security: more roboust security features and security policies.

Logging and Auditing: having detailed logging and auditing capabilities to track changes 

Notifications: having notification services (email,slack)

Analytics: Advanced analytics and reporting capabilities.

Support: Enterprise grade support from Redhat

## Getting started with AWX/ Tower

1.Install aws/Ansible tower based on env(docker,k8s...)

2.Set up Inventory includinh hosts and groups

3.create projects: link your playbooks repository to a project in AWX/Tower

4.Create a job template to specify a playbbok to run and the parametrs to use.

5.Add credentials to securely accessing

6.Excute jobs manually or ajtomatically

7.use dash bpard , job views, and logs to monitor and manage your automation tasks.

```
What are the features of the Ansible Tower?
Features of the Ansible Tower are:

Ansible Dashboard
Real-time job status updates
Multi-playbook workflows
Who Ran What Job When
Scale capacity with tower clusters
Integrated notifications
Schedule ansible jobs
Manage and track inventory
Remote command execution
REST API & Tower CLI Tool.
```

