# JMeter VM setup with Ansible

This repository contains all tools necessary to generate some Amazon Lightsail VMs to do load testing.

First of all you need to install [ansible](http://docs.ansible.com/ansible/latest/intro_installation.html), [awscli](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) and some extra dependencies.

On a Ubuntu 16.04 system you can do that by executing the following :

    sudo apt-get update
    sudo apt-get install software-properties-common
    sudo apt-add-repository ppa:ansible/ansible
    sudo apt-get update
    sudo apt-get install ansible
    
    pip install awscli
    
    pip install boto3
    
Make sure you setup awscli to connect with your Amazon credentials by executing :

    aws configure

And finally create a keypair to be used for the Lightsail VMs & upload the public key to Lightsail :

    ssh-keygen -t rsa -b 4096 -C "jmeter@domain.com" -f ~/.ssh/id_jmeter

Once everything is setup correctly you should be able to create the VMs that are needed.

A test setup that is included (in vars.yml.dist) will create 3 VMs, one is the client, the other VMs are jmeter servers. You can create as many as you want.

**NOTE**: You should copy vars.yml.dist to vars.yml and edit it to fit your needs, make sure you use the correct key pair! If you do not use the AWS ubuntu image, you might have to adapt the playbooks as well ...

To create the VMs execute the following :

    ansible-playbook create-vm.yml

Once all VMs are actually running (check on the [AWS Lightsail dashboard](https://lightsail.aws.amazon.com/ls/webapp/home/resources)) you can provision all VMs by executing the following :

    ./provision.sh

To destroy the VMs (after running the load test) execute the following :

    ansible-playbook destroy-vm.yml
