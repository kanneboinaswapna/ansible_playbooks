## creating roles from playbook

## identify playbook sections
such as tasks,handlers,variables,templates,files...
## create role directory structure
```
roles/
└── your_role/
    ├── tasks/
    │   └── main.yml
    ├── handlers/
    │   └── main.yml
    ├── vars/
    │   └── main.yml
    ├── defaults/
    │   └── main.yml
    ├── files/
    ├── templates/
    ├── meta/
    │   └── main.yml
    └── README.md
```
## move playbook sections to role
```
Tasks: Move your tasks to roles/your_role/tasks/main.yml.
Handlers: Move your handlers to roles/your_role/handlers/main.yml.
Variables: Move variables to roles/your_role/vars/main.yml or roles/your_role/defaults/main.yml.
Files: Move any files to roles/your_role/files/.
Templates: Move any templates to roles/your_role/templates/.
Meta: Define role dependencies and other metadata in roles/your_role/meta/main.yml.
```
## Update the Main Playbook
```
- hosts: your_hosts
  roles:
    - your_role
```

```
- hosts: webservers
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```


