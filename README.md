# ansible
Ansible is an open source configuration management tool, that can automates provisioning, configuration management,application deployment,orchestration.

ansible is agentless , no need to install any other softwares on manage nodes

it suuports linux,, windows os whilw shell scripting is only for linux systems

ansible is idompotence in nature , there is no additional 
changes even you run playbook multiple times.

it is very important large and complex IT environments where maintaning contistence,reliability,security is challenging.

### INVENTORY:
in ansible inventory is a fundamental component that defines hosts (remote systems)that can manage collection of nodes.The inventory file can be static (simple text file) or dynamic (generated by a cript). It provides ansible with the information about the remote nodes to communicate with the during its operations.
### static inventory
plain text fileand structired in INI or YAML format
""" inventory file: hosts

[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
db2.example.com

[all:vars]
ansible_user=admin
ansible_ssh_private_key_file=/path/to/key
### yaml
inventory file: hosts.yaml

all:
  vars:
    ansible_user: admin
    ansible_ssh_private_key_file: /path/to/key
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    dbservers:
      hosts:
        db1.example.com:
        db2.example.com: """
