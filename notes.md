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



