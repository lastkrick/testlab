# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/fedora33"
  config.vm.box_check_update = true

  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  # SHELL

  config.vm.provision "shell", inline: "echo run-on-all"
  (3..5).each do |i|
    config.vm.provision "shell", inline: "sed -i '/192.168.50.#{i}/d;$a 192.168.50.#{i} node#{i}.vm.net' /etc/hosts"
  end
  
  (3..5).each do |i|
    config.vm.define "node#{i}" do |vb|
      vb.vm.provision "shell", inline: "echo this-is-node#{i}"
      vb.vm.network "private_network", ip: "192.168.50.#{i}"
      vb.vm.hostname = "node#{i}.local.net"
    end
  end
end
