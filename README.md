# ansible-project

### Create Host file
#### vi hosts.ini
```
web ansible_host=ec2-3-109-214-121.ap-south-1.compute.amazonaws.com ansible_port=22 ansible_user=ec2-user ansible_ssh_private_key_file=/home/ec2-user/my-key.pem
```

### test connection using ping module
### ansible -m ping <host-name> -i host-file
```
ansible -m ping web -i hosts.ini
```

### Create Playbook
#### Ref
```
https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#playbooks-intro
https://docs.ansible.com/ansible/latest/modules/apache2_module_module.html
```

### Handlers
#### Ref https://docs.ansible.com/ansible/latest/user_guide/playbooks_handlers.html

### to execute shell command in /bin/bash
```
- name: Creating a file with content
    shell: echo "this is ansible" > /var/www/html/index.html
    args:
        executable: /bin/bash
```

### To get the output from command
#### Register
```
- name: Curl AWS
      shell:
        cmd: curl http://169.254.169.254/latest/meta-data/public-ipv4
      register: curl
    - debug: var=curl.stdout_lines
```
**************
## varible file

### To get the value from vars.yaml file into our playbook
#### Playbook.yaml
```
- hosts: localhost
  vars_files:
    - vars.yaml
  tasks:
    - name: Print name variable
      debug:
        msg: "{{ name }}"
```
### When vars_file not mention in playbook, then pass it in command
```
ansible-playbook variable-playbook -e "@vars.yaml" -i hosts.ini
```
### To run loop in module
#### Refer loop.yaml file

### To get array of varibale to select in module
```
- hosts: web
  remote_user: root
  become: yes
  vars:
    courses:
     - httpd
     - git
     - mysql
     - nano
  tasks:
    - name: My fav course is.
      debug:
        msg: "My fav course is {{ courses [2] }}

```

**********************************
## Role 

### To create role 
```
ansible-galaxy init <role name>
```

### For multiple role from files
```
ansible-galaxy install --roles-path roles/ -r roles/requirement.yaml
ansible-galaxy collection install -p roles/ -r roles/requirement.yaml
```
#### requirement.yaml
```
roles:
  - src: https://github.com/companieshouse/ansible-role-apache
    name: apache-ch
  - name: geerlingguy.docker

collections:
  - geerlingguy.k8s
```
### Varibales
#### Priorities wise
1. variable passed with command -e "HTTP_PORT=80"
2. varibale define in vars folder
3. varibale define in default folder

### Define dependencies for role on other role
#### Mention the name of the role in dependencies section of mata folder
***********************************************
## Dynamic host file

### More information on Dynamic Inventories: 
https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html

### Link to ec2.py: 
https://raw.githubusercontent.com/ansible/ansible/stable-1.9/plugins/inventory/ec2.py

### Link to ec2.ini: 
https://raw.githubusercontent.com/ansible/ansible/stable-1.9/plugins/inventory/ec2.ini
**********************************************
## Connect with Window remote host
### Create hostfile
```
window anisble_host=172.31.35.73 ansible_user=administrator ansible_password=)U*tFC9*EhJ2zU5LI2!7DQN*-4zBjx-l ansible_connection=winrm ansible_winrm_scheme=https ansible_port=5986 ansible_winrm_server_cert_validation=ignore
```
### winrm setup in remote server
#### Ref https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#host-requirements
### Run playbook
```
ansible-playbook -i window-host.ini -m win_whoami
ansible-playbook -i window-host.ini -m win_ping
```
*******************************************
## Ansible Vault
### To encrypt the file
```
ansinle-vault encrypt <file name>
```
### To view the encypted fiel
```
ansible-vault view <file name>
```
### Play the playbook with encrypted file hosts.ini
```
ansible-playbook playbook.yaml -i hosts.inin --ask-vault-pass
```
### chenge the encryption key
```
Change the password of the encrypted vault file
```
### To change the encryption key
```
ansible-vault rekey hosts.ini
```
### to decrypt the file
```
ansible-vault decrypt <file-name>
```
*************************************************
## Hasicorp Vault

### Link to install Vault: https://learn.hashicorp.com/tutorials/vault/getting-started-install

### Link to the documentation for the vault module: https://docs.ansible.com/ansible/latest/plugins/lookup/hashi_vault.html

### If you run into: in progress in another thread when fork() was called

Run:
```
export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```
Then re-run the command

### Install python lib hvac
pip install hvac

Ansible mast install through pip

vault-demo.yaml
```
- hosts: localhost
  vars:
    vault_token:
    vault_url:
    db_password:
    system_password:
  tasks:
    - name: Value for DB password
      debug:
        msg: "db Passowrd {{ db_password }}"
    - name: Value for system password
      debug:
        msg: "db Passowrd {{ system_password }}"
```
### developer mode, not recommended for prod
vault server -dev 

vault secret enable -version=1 -path=secret kv

vault kv put secret/password/db data=dbpass123

vault kv put secret/password/system data=systempass123

ansible-playbook -i hosts.ini vault-demo.yaml