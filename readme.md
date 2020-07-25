# Ansible

## Step By Step Details
- Step 01 - Creating EC2 Instances for Ansible - Manually and with Terraform
- Step 02 - Setting Ansible Project with cfg and ansible hosts
- Step 03 - Playing with Ansible Commands
- Step 04 - Playing with Ansible Host File and Custom Groups
- Step 05 - Creating an Ansible Playbook for Ping
- Step 06 - Understanding Ansible Terminology - Control Node, Managed Nodes, Inventory
- Step 07 - Creating New Ansible Playbook for Executing Shell Commands
- Step 08 - Playing with Ansible Variables
- Step 09 - Creating New Ansible Playbook for Understanding Ansible Facts
- Step 10 - Creating New Ansible Playbook for Installing Apache and Serving HTML
- Step 11 - Reuse and Executing Multiple Ansible Playbooks
- Step 12 - Understanding Conditionals and Loops with Ansible
- Step 13 - Configuring EC2 Dynamic Inventory with Ansible
- Step 14 - Creating AWS EC2 Instances with Ansible
- Step 15 - Providing Declarative Configuration with Ansible
- Step 16 - Deleting all AWS EC2 Instances

### Prerequisites
- 3 EC2 Instances 
    - 2 using Terraform
    - 1 Manually
    - You can use which ever approach you are comfortable with

- EC2 Keys - `ls ~/aws/aws_keys/default-ec2.pem`

- AWS CLI - `aws configure`
```sh
# or
export AWS_ACCESS_KEY_ID=**************
export AWS_SECRET_ACCESS_KEY=**************
```

- boto3 and botocore - For EC2 Dynamic Inventory and Creating EC2 Instances
```sh
# TEst
python
# boto3 quick start
> import boto3
> client = boto3.client('ec2')
```

### Create EC2 Instances using Terraform

```
cd terraform/backup/09-multiple-ec2-instances
export AWS_ACCESS_KEY_ID=**************
export AWS_SECRET_ACCESS_KEY=**************
terraform init
terraform apply
ls ~/aws/aws_keys/ # Make sure that the keys file is present
```

### Ansible Commands

```
cd /in28Minutes/git/devops-master-class/ansible 
ansible --version
ansible -m ping all 
ansible all -a "whoami"
ansible all -a "uname"
ssh -vvv -i ~/aws/aws_keys/default-ec2.pem ec2-user@54.255.14.89
ls ~/aws/aws_keys/default-ec2.pem
chmod 400 /Users/laymui/aws/aws_keys/default-ec2.pem
ansible all -a "uname"
ansible all -a "uname -a"
ansible all -a "pwd"
ansible all -a "python --version"
ansible dev -a "python --version"
ansible qa -a "python --version"
ansible first -a "python --version"
ansible groupofgroups -a "python --version"
ansible devsubset -a "python --version"
ansible --list-host all
ansible --list-host dev
ansible --list-host first
ansible --list-host \!first
ansible --list-host qa:dev

ansible-playbook playbooks/01-ping.yml
ansible-playbook playbooks/02-shell.yml 
ansible-playbook playbooks/03-variables.yml 
ansible-playbook playbooks/03-variables.yml -e variable1=CommandLineValue
ansible-playbook playbooks/04-ansible-facts.yml 
ansible-playbook playbooks/05-install-apache.yml 
ansible-playbook playbooks/06-playbooks.yml 
ansible-playbook playbooks/06-playbooks.yml --list-tasks
ansible-playbook playbooks/06-playbooks.yml --list-hosts
ansible-playbook playbooks/06-playbooks.yml --list-tags
ansible-playbook -l dev playbooks/01-ping.yml
ansible-playbook playbooks/07-conditionals-and-loops.yml 
ansible-inventory --list
ansible-inventory --graph
ansible-playbook playbooks/08-dynamic-inventory-ping.yml 
ansible-playbook playbooks/09-create-ec2.yml 

```

Steps:
```
create the new files:
ansible.hosts
ansible.cfg
create new folder called playbooks to store all the ansible scripts here

01: Start ansible configuration at ansible.cfg
[defaults]
inventory in the ansible_hosts
remote_user what is the name of the ssh we want to config, in EC2, the default user is ec2-user
private_key_file: to able to talk to EC2 instance, we need this key file
To prevent question to pop up, set host_key_checking=false
Whenever there is a failure, there is retry files

02: Config server with ansible.hosts
Go to AWS and copy over the public IP of the EC2 instances

ansible -m ping all: to check if we can ping those EC2 servers
ansible dev -a "python --version" where dev is defined in the ansible.hosts under dev group/section

ansible --list-host \!first -> anything that is not first