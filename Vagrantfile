# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  groups = {
    "master" => ["master"],
  }

  config.vm.box = "ubuntu/bionic64"

  config.vm.define "master" do |master|
    master.vm.network "private_network", ip: "192.168.100.10"

    master.vm.provision "ansible_local" do |ansible|
      ansible.verbose = "vvv"
      ansible.playbook = "site.yml"
      ansible.become = true
      ansible.groups = groups
    end
  end

end
