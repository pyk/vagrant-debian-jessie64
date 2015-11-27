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
on the `Vagrantfile`.
    

[ansible]: https://ansible.com