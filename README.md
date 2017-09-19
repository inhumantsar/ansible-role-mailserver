# ansible-role-mailserver

## What?

Stands up a mailserver as a Docker Compose based service.

## Warnings
* This is *not* idempotent and it's a bit janky. 

## How?
```bash
yum clean all && yum update -y
yum install -y git gcc python-devel openssl-devel

# install pip - rhel only
# curl --silent --show-error --retry 5 https://bootstrap.pypa.io/get-pip.py | sudo python2.7

# workaround for rhel systems w/o subscription access
# rpm -ivh http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.10-2.el7.noarch.rpm

pip install ansible

echo -e """- src: inhumantsar.mailserver""" > requirements.yml

ansible-galaxy install -r requirements.yml

echo """---    
- hosts: localhost
  roles:
   - inhumantsar.mailserver""" > playbook.yml

ansible-playbook playbook.yml
```

### Decommission
```bash
ansible-playbook playbook.yml -e mailserver_state=absent```
