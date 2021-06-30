# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["VAGRANT_EXPERIMENTAL"]="disks,dependency_provisioners"

disk_filename_prefix = "./disk_node"
disk_filename_postfix = ".vdi"
ip_addr_prefix = "192.168.24"

Vagrant.configure("2") do |config|
  config.vm.box = "generic/fedora33"
  config.vm.box_check_update = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: "echo run-on-all"
  (3..5).each do |i|
    config.vm.provision "shell", inline: "sed -i '/#{ip_addr_prefix}.#{i}/d;$a #{ip_addr_prefix}.#{i} node#{i}.vm.net' /etc/hosts"
  end
  
  config.vm.provision "playbook", after: :all, type: "ansible" do |ans|
    ans.playbook = "playbook.yml"
    ans.limit = "all"
  end

  (3..5).each do |i|
    config.vm.define "node#{i}" do |vb|
      vb.vm.provider :virtualbox
      vb.vm.provision "shell", inline: "echo this-is-node#{i}"
      vb.vm.network "private_network", ip: "#{ip_addr_prefix}.#{i}", virtualbox__intnet: true
      vb.vm.hostname = "node#{i}.local.net"
      disk_filename = "#{disk_filename_prefix}#{i}#{disk_filename_postfix}"
      vb.vm.disk :disk, size: "4GB", name: disk_filename
    end
  end
end
