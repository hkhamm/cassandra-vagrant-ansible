cassandra-vagrant-ansible
=========================

This directory includes a Vagrant and an Ansible playbook that install and setup Cassandra along with all necessary third party components, include Oracle Java 7.

Vagrant Project using an Ansible playbook for installing Oracle Java 7 and Datastax Cassandra 2.0.


Running the Ansible
-------------------

Open a command line instance and enter this command:

```
ansible-playbook <hosts> main.yml -u <username>
```

Running the vagrant
-------------------

Navigate to the same level as VagrantFile.

Create the vagrant:
```
vagrant up
```

SSH into the vagrant:
```
vagrant ssh
```
