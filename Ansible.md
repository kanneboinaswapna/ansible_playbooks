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
```
pip install ansible```
or 
```
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
make playbook.yaml```

## Building inventory
```
[hosts]
192.0.2.50
192.0.2.51
192.0.2.52
```
check-- ansible hosts -m ping -i <inventory file name>
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
    x.x.x.x:```

# Creating playbook
Playbook is a Yaml file, that can be series of tasks that are excute on remote systems.
It is order of operations top to bottom overall a goal

## Tasks
single module operation in ansible
## Module


   
