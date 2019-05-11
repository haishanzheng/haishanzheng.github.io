# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.network "forwarded_port", guest: 22, host: 6922, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 4000, host: 6940, host_ip: "127.0.0.1"

#  config.vm.synced_folder ".", "/var/www/wwwxmu"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false
  
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
    vb.name = "doghomepage"
  end
end
