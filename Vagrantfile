# -*- mode: ruby -*-
# vi: set ft=ruby :
file_to_disk = './disk1.vdi'
Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos8"
  config.vm.provider :virtualbox do |vb|

    vb.customize ['createhd', '--filename', file_to_disk, '--size', 1000]
    vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
    vb.customize ['storageattach', :id, '--storagectl', 'SATA', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
  end
    config.vm.provision "shell", inline: <<-SHELL
    mkdir -p ~root/.ssh
          cp ~vagrant/.ssh/auth* ~root/.ssh
          
  SHELL
  
end


