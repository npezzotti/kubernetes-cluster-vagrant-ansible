# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IMAGE = "ubuntu/xenial64"
NODE_COUNT = 1

Vagrant.configure("2") do |config|
  config.vm.define "master", primary: true do |master|
    master.vm.box = BOX_IMAGE
    master.vm.hostname = "master"
    master.vm.network :private_network, ip: "10.0.0.10", hostname: true
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "kubernetes-setup/master-playbook.yaml"
    end
    master.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 2
    end
  end
  
  (1..NODE_COUNT).each do |i|
    config.vm.define "node-#{i}" do |worker|
      worker.vm.box = BOX_IMAGE
      worker.vm.hostname = "node-#{i}"
      worker.vm.network :private_network, ip: "10.0.0.#{i + 10}", hostname: true
      worker.vm.provision "ansible" do |ansible|
        ansible.playbook = "kubernetes-setup/node-playbook.yaml"
      end
      worker.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
      end
    end
  end
end
