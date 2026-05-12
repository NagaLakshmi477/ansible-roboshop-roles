Ansible Roles:
==============
DRY ---> don't repeat your self

varibales 
functions

Ansible we have lot of common code:

create system user
install programming language
download the code
create directory
unzip code
install dependies
create systemctl services
start services

roles is a proper directory structure to write ansible playbooks, we can re use roles


MONGODB:
=========
for mongodb we need to create role
role is a directory 
here we can dynamically give the component names in main.yaml file
command:
ansible-playbook -i inventory.ini -e component=mongodb main.yaml

Catalogue:
=============

Here we have created catalogue floder under roles
catalogue---> task ---> main.yaml 
Here we can call another role in the main.yaml file
common ---> task ---app-setup.yaml ---> It contain common code
this is node js application so we can keep node js in nodejs.yaml file
we can place the repo file under catalogue ----> files ---> mongo.repo

Template:
=========
we can keep some place holders we can put some values at runtime

file:
=====
content inside the specfic file

Ansible templates:
==================
follows jinja2 formatting we can keep some placeholders, actual values will be provided throught variabkes at run time

we will change the service file MONGODB_HOST
for we have catalogue-service.j2 
and for giving values we have vars ---> main.tf


tasks
    main.yaml ---> playbook related task are here
files
    <file-name> ---> you can keep all the files required here
templates
    <template-file> ----> we can keep the all templates with placehloders here. usually we fallow jinja2 templating variable values can be suppiled
vars
    main.yaml ---> all variables required for the roles can be kept here


Frontend:
=========
Here we need to restart the application ---> if any changes in configuration file
for this we have handlers
by deafault it is main.yaml

Handlers:
----------
handlers are the notifiers in ansible. when there is a change in something if you want to notify other task we can use handlers

Ansible tags:
================
deployment or new version is released 

we will announce down time for 6 hrs
------------------------------------
ex: catalogue

stop catalogue service
remove old code
download new code
restart catalogue

common ---> tasks ---> deployment.yaml

we can include other roles using include_role or import_role

include_role:
------------
 it will include the tasks in run time it will validate also runtime.
it will not pre processed

import_role: 
------------
ansible validates import role before playbook execution.
Tags and when conditions are apply to the imposrted role and tasks

Ansible error handling:
=======================
Error handling means what should we do when errors comes what we should do when error is not comes

if we are able to handle the error we can keep ignore_error: true
and then execute another task in case of failures

in shell:
---------
id robsohop:
        if created already we are skipping
        otherwise we will create

imagine there is no user module is available

a task executes id roboshop ---> tasks fails if user is not available ---> usually scrpit exit beacuse of this error --> but we already know this will exit so we need to handle this one
another task create user, if rc != 0

 ignore_errors: true ---> this is we will use


 Vault:
 =======
 password mangement is poor in ansible
you can create file and encrypt it

 to create ansible vault ----> ansible-vault create test.yaml
 to view the password ----> ansible-vault view test.yaml
 to edit the password -----> ansible-vault edit test.yaml

now we will create a vault floder

 vauts are not recommenede to push into github
 we need to create .gitignore file under the vault floder

we need to maintain the passwords in server

 After pulling the latest we will not get the vault floder (we used .gitignore)
 so we need to create the vault in server

 mkdir vault
 cd vault
 ansible-vault create mysql.yaml

 MYSQL_ROOT_PASSWORD: RoboShop@1
 
 SSM parameter store (ssm)
 -------------------
 linux ---> server
 Ansible ---> platform

 Hashicrop ---> install, manage, upgrade, update 
 linux/ansible integrate with vault software

 AWS ---> SSM parametr ---> parameter store ---> create parameter ---> need to start with / ---->/roboshop/mysql/mysql_root_password ---> secure string --> RoboShop@1

 How to connect aws cli:
 ------------------------
 ansible is developed using python.
 we need to install some packages to connect aws (boto3 and boto core)
 Autonications is also required

 we are using ansible vault but there are few challanges to maintain the passwords and files.
 I recently propsed SSM parameter store I did POC i removed vault and implatemented ssm parameter sucessfully



 Ansible-dynamic Inventory:
 ------------------------
 Till now we used sttatuc inventory file  ---> IP address and hosts are fixed

Auto scaling: ----> when tarffic increse infra also increse when traffic descrese infra alos decrease
ansible queries the ec2 instances based on some critiria.

now ansible needs to connect with ec2:
--------------------------------------
we need to install boto3 and botocore

filter:
us-east-1
running instances
name ---> frontend

file name should be ---> *.aws_ec2.yaml/yml 
plugin: adding extra functionlity to our code . it came from installltions(boto)

how ansible parallely connects to the ec2 instance:

craete a file in ansible repo 
how to execute the file:

ansible-inventory -i inventory.aws_ec2.yaml --graph
--------------------------------------------------
using forks ---> how many number of instances ansible can connect


[ ec2-user@ip-172-31-32-218 ~/ansible ]$ ansible --version
ansible [core 2.14.18]
  config file = /home/ec2-user/ansible/ansible.cfg
  configured module search path = ['/home/ec2-user/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /home/ec2-user/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.9.18 (main, Jan  4 2024, 00:00:00) [GCC 11.4.1 20230605 (Red Hat 11.4.1-2)] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True

we will create
--------------
cd ..
mkdir config
cd config
cd ..

cp /etc/ansible/ansible.cfg config
now we will check ansible version

Here i don't want to load the deafulaty configuration 
i want to load the ansible configuration
for this i want to create environementy variables

# export ANSIBLE_CONFIG=/home/ec2-user/config/ansible.cfg
now check the version
ansible --version

 config file = /home/ec2-user/config/ansible.cfg

unset ANSIBLE_CONFIG
ansible --version
 config file = /etc/ansible/ansible.cfg

- It will check is any user create that .cfg file ---> 1st 
ANSIBLE_CONFIG ---> 1st preference

# now i will create file in ansible floder 
ansible.cfg ----> in the current directory
cd ansible
ansible --version

config file = /home/ec2-user/ansible/ansible.cfg

# 3rd ~/.ansible.cfg ---> in the home directory
pwd
/home/ec2-user
cp /etc/ansible/ansible.cfg .ansible.cfg
ansible --version 
  config file = /home/ec2-user/.ansible.cfg



/etc/ansible/ansible.cfg ----> last prefernce
ANSIBLE_CONFIG=/home/ec2-user/config/ansible.cfg
export ANSIBLE_CONFIG=/home/ec2-user/config/ansible.cfg
ansible --version

ansible-serial
------------
ex: we will take serial is 4 
so playbook run the play for every 4 instances(onetime) again 8th server like this

ansible can query the cloud provider to fetch hosts at run time this will useful in the autoscaling kind of dyanamic envi

aws can create aws infra
---------------------
ansible is very poor in state management
for ansible everything is module if module is available it can create anything. Ansible may require libraries . to connect we need boto3 and botocore. aws configuration too

we will create
1. aws instances
2. route 53 records


keybased instances:
------------------
we need to create a key and login ec2 ---normal (not rhel) -> using key
login to
$ ssh -i ramesh ec2-user@18.232.106.12

here we are able to connect using ssh means we can able to connect to ansible also beacuse it is also working using ssh
means we need keys for ansible also and we need to store keys in server
login into ansible
mkdir keys
cd keys
vim <key-file.pem>

cop ---> public key (bash) --- paste ---> keys.pem ---> save

the max permissions are 600 
chmod 600 <file>
 ssh -i ramesh.pem ec2-user@172.31.38.203(private-ip)

 now we able to connect so we can mention the pem file location in nventory file
we can one file like keys.yaml 


 
