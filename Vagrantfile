# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/fedora33"
  config.vm.box_check_update = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: "echo run-on-all"
  (3..5).each do |i|
    config.vm.provision "shell", inline: "sed -i '/192.168.50.#{i}/d;$a 192.168.50.#{i} node#{i}.vm.net' /etc/hosts"
  end
  
  config.vm.provision "ansible" do |ans|
    ans.playbook = "playbook.yml"
    ans.limit = "all"
  end

  (3..5).each do |i|
    config.vm.define "node#{i}" do |vb|
      vb.vm.provision "shell", inline: "echo this-is-node#{i}"
      vb.vm.network "private_network", ip: "192.168.50.#{i}"
      vb.vm.hostname = "node#{i}.local.net"
      disk_filename = "./additional_disk_node#{i}.vdi"
      #vb.vm.provider "virtualbox" do |p|
      #  unless File.exist?(disk_filename)
      #    p.customize ['createhd', '--filename', disk_filename, '--size', 4*1024]
      #    p.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk_filename]
      #  end
      #end
    end
  end
end
