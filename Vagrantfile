# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    # Setup the VM
    config.vm.box = "ubuntu/trusty64"
    config.vm.box_url = "https://vagrantcloud.com/ubuntu/trusty64"
    # Install Oracle Java 7 and Datastax Cassandra 2.0
    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "setup.yml"
    end
end
