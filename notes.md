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

if we are able to handle the error we can keep ignore_error: true and then execute another task in case of failures

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
 
 SSM parameter store
 -------------------
 linux ---> server
 Ansible ---> platform

 Hashicrop ---> install, manage, upgrade, update 
 linux/ansible integrate with vault software

 AWS ---> SSM parametr ---> parameter store ---> create parameter ---> need to start with / ---->/roboshop/mysql/mysql_root_password ---> secure string --> RoboShop@1