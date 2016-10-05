# -*- mode: ruby -*-
# vi: set ft=ruby :

###########################################################
# Based on https://docs.chef.io/install_delivery_ssh.html #
###########################################################

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.synced_folder ".", "/vagrant/"
  config.vm.provision "shell", inline: "cp /vagrant/id_rsa* /root/.ssh/ && cp /vagrant/id_rsa.pub /root/.ssh/authorized_keys"


  config.vm.define "Delivery Server" do |delivery_server|
    delivery_server.vm.network "private_network", ip: "192.168.50.5"
    delivery_server.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "16384"]
        vb.customize ["modifyvm", :id, "--cpus", "4"]
    end
  end

  config.vm.define "Chef Server" do |chef_server|
    chef_server.vm.network "private_network", ip: "192.168.50.6"
    # .pem creation was a problem possibly caused by missing entropy -> install haveged
    chef_server.vm.provision "shell", inline: "apt-get update && apt-get install -y haveged"
    chef_server.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "8192"]
        vb.customize ["modifyvm", :id, "--cpus", "4"]
    end
  end

  config.vm.define "Build Node" do |build_node|
    build_node.vm.network "private_network", ip: "192.168.50.7"
    build_node.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "4096"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end

  config.vm.define "Provisioning Node" do |provisioning_node|
    provisioning_node.vm.network "private_network", ip: "192.168.50.100"
    provisioning_node.vm.provision "shell", inline: "apt-get update && apt-get install -y build-essential git"
    provisioning_node.vm.provision "shell", inline: "wget https://packages.chef.io/stable/ubuntu/12.04/chefdk_0.13.21-1_amd64.deb && dpkg -i chefdk_0.13.21-1_amd64.deb"
    provisioning_node.vm.provision "shell", inline: "git config --global user.name Test && git config --global user.email johndoe@example.com"
    provisioning_node.vm.provision "shell", inline: "cp /vagrant/delivery.license /root/"
    provisioning_node.vm.provision "shell", inline: "git clone https://github.com/opscode-cookbooks/delivery-cluster.git ~/delivery-cluster"
    provisioning_node.vm.provision "shell", inline: "mkdir ~/delivery-cluster/environments && cp /vagrant/test.json ~/delivery-cluster/environments/test.json"
    provisioning_node.vm.provision "shell", inline: 'export PATH="/opt/chefdk/embedded/bin:$PATH" && CHEF_ENV=test && cd ~/delivery-cluster && rake setup:cluster'
    provisioning_node.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end

end
