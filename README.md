# ansible-project

Create Host file

test connection using ping module
ansible -m ping host-name -i host-file
```
ansible -m ping web -i hosts.ini
```

Create Playbook
Ref https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#playbooks-intro
https://docs.ansible.com/ansible/latest/modules/apache2_module_module.html

Handlers
Ref https://docs.ansible.com/ansible/latest/user_guide/playbooks_handlers.html

shell module to run shell command

Register is use to save the output from the command to use further in playbook

varible file

to get the value from vars.yaml file into our playbook
run command
ansible-playbook variable-playbook -e "@vars.yaml" -i hosts.ini

Role 

to create role 

ansible-galaxy init <role name>