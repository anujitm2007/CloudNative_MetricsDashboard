# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"
  
  config.vm.hostname = "anuj-machine"
  config.vm.box_check_update = false

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # Author's Comment: I have used IP of my choice here.
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.ssh.username = "vagrant"
  config.ssh.private_key_path=["~/.vagrant.d/insecure_private_key"]
  config.ssh.insert_key = false
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "4096"
    vb.cpus = "4"
    vb.name = "anuj-machine"
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end
  config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  SHELL
end
