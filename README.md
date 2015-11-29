# debian - Jessie Machine
This machine is used as mirror of the production server. Provision, deployment 
or continuos integration should be done using tools like ansible, gitlabs or 
drone.

## Setup Ansible on Host machine
The best way to manage this machine is via [Ansible][ansible].

Install Ansible on host machine

    sudo apt-get install software-properties-common
    sudo apt-add-repository ppa:ansible/ansible
    sudo apt-get update
    sudo apt-get install ansible

Then setup the inventory for ansible.

    sudo cp /etc/ansible/hosts /etc/ansible/hosts.orig

Add this line to `/etc/ansible/hosts`

    [local]
    192.168.33.10

make sure this address are match with `jessie.vm.network "private_network"`
on the `Vagrantfile`. Make sure it works
    
    $ ansible all -m ping -s -k -u vagrant
    SSH password: 
    192.168.33.10 | success >> {
        "changed": false, 
        "ping": "pong"
    }
    
[ansible]: https://ansible.com

## Install basic package
Install baisc package like `vim` and `git`

    ansible-playbook -s -k -u vagrant ansible/nginx.yml

## Install nginx
To install nginx run the following command

    ansible-playbook -s -k -u vagrant ansible/nginx.yml

## Install postgresql
To install postgresql run the following command

    ansible-playbook -s -k -u vagrant ansible/postgresql.yml

User management and database management should be handled manually

    # login to machine
    vagrant ssh
    # switch to postgres user
    sudo -u postgres -i
    # create database
    createdb dbname
    # create role to access database
    createuser -E -l -P rolename

Now assign privilege to the role `rolename` to read & write to the 
database `dbname`

    # run this as postgresql user
    psql
    # inside psql
    grant all privileges on database dbname to rolename;   

Press `CTRL+D` to logout as `postgres` user. 
Edit `/etc/postgresql/9.4/main/pg_hba.conf`
    
    sudo vim /etc/postgresql/9.4/main/pg_hba.conf

Change line
    
    local   all             all                                     peer

to

    local   all             all                                     md5

Reload postgresql service
    
    sudo systemctl reload postgresql

Now you can access database `dbname` via `rolename`

    psql -U rolename -d dbname

## Install Gitlab on local machine
Install via ansible playbook

    ansible-playbook -s -k -u vagrant ansible/gitlab.yml

Then add this line to `/etc/hosts` on host machine.

    192.168.33.10 gitlab.sayoeti.box

Access [gitlab.sayoeti.box](http://gitlab.sayoeti.box) via browser on host 
machine, It may take a minutes to gitlab up and running. Login using `root`
and `5iveL!fe`.