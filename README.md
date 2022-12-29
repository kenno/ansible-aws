
# Ansible playbook to create EC2 instance

An ansible playbook which can be used to launch an EC2 instance.

The playbook will create the following items in order:

* A key-pair it it doesn't exist
* A securigy group, with a predefined rule to allow SSH access from 0.0.0.0/0
* An EC2 instance

Inspired by a [playbook](https://github.com/davelms/medium-articles/tree/master/ansible-aws) by Dave Sugden.

## Why was this playbook created?

I need a playbook which I can use to quickly launch and instance. Sure, we can manually use `aws ec2` command to achieve all these tasks or write a bash script to do that. However, I personally like Ansible and already use it to manage many other things, so using to manage AWS resource fits my use case naturally.

## What do I need before I can run this playbook?

You need to install Ansible on your machine, aka the control node. Follow the official [Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-specific-operating-systems).

Here an example on instaling Ansible on a macOS using homebrew:

```
❯ brew install ansible
```

Additional requirements:

* boto
* boto3 >= 1.15.0
* botocore >= 1.18.0
* python >= 3.6

On a macOS, I install the above Python package using `pip3`. You can try other methods, if that didn't work for you.

```
❯ pip3 install boto
❯ pip3 install boto3

❯ pip3 list | grep boto -B1
--------------- -------
boto            2.49.0
boto3           1.21.19
botocore        1.24.19
```

## How to run this playbook?

Assuming that you have configured AWS CLI with correct permission to launch an instance, you can run the following command to create an instance with default parameters (defined in `defaults/main.yml`):


```
❯ git clone https://github.com/kenno/ansible-aws
❯ cd ansible-aws
❯ ansible-playbook create-ec2.yml -i inventory
```

Here is another example of creating an instance in eu-west
Please check the default variables set in `defaults\main.yml`.

```
REGION='eu-west-1'              # Ireland
AMI="ami-002d3240b69a0ef4e"     # RHEL-7.9_HVM-20220222-x86_64-0-Hourly2-GP2 -  (eu-west-1)
NAME='rhel7.9-nfs-client'
KEY_NAME="my-${REGION}"
SG_NAME="rhel7.9-sg"

❯ ansible-playbook create-ec2.yml -i inventory \
    -e "instance_type=t2.micro instance_name=${NAME} \
    ami_id=${AMI} region_name=${REGION} \
    key_name=${KEY_NAME} security_group_name=${SG_NAME}"
```
