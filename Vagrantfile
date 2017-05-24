# -*- mode: ruby -*-
# vi: set ft=ruby :
ENV["LC_ALL"] = "en_US.UTF-8"
$nodes = 1

Vagrant.configure(2) do |config|

 (1..$nodes).each do |i|
     config.ssh.username = "vagrant"

     config.vm.define "node#{i}" do |node|
         node.vm.box = "ubuntu/trusty64"
         node.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
         node.vm.network :public_network, ip: "10.150.0.#{200+i}",
            :use_dhcp_assigned_default_route => true, bridge: "enp2s0f0"
         node.vm.network :private_network, ip: "192.168.111.#{10+i}"
         node.vm.hostname = "node#{i}"
         node.vm.synced_folder '.', '/vagrant', mount_options: ["dmode=777,fmode=666"]

     node.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 1
       end

           if i == $nodes
               node.vm.provision :ansible do |ansible|
                  #  ansible.verbose = "vvv"
                   ansible.limit = "main"
                   ansible.playbook = "main.yml"
                   ansible.inventory_path = "./asterisk-inventory.ini"
               end
           end
        end
    end
end
