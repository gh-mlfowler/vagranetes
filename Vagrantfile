# -*- mode: ruby -*-
# vi: set ft=ruby :

# ====================
# BOX DEFINITIONS
# ====================

DEFINITIONS = [
  {
    name: "master1",
    memory: 2048
  },
  {
    name: "worker1",
    memory: 2048
  },
]

Vagrant.configure(2) do |config|

  groups = {
    "master" => ["master1"],
    "worker" => ["worker1"],
  }

  config.vm.box = "ubuntu/bionic64"

  ssh_base_port = 2500
  DEFINITIONS.each_with_index do |definition,idx|
    name = definition[:name]

    config.vm.define name do |c|

      # == Basic box setup
      c.vm.hostname = "vagrant-#{name}"

      c.vm.network :public_network

      # == Disable vagrant's default SSH port, then configure our override
      new_ssh_port = ssh_base_port + idx
      c.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", disabled: "true"
      c.ssh.port = new_ssh_port
      c.vm.network :forwarded_port, guest: 22, host: new_ssh_port

      # == Set resources if configured
      c.vm.provider "virtualbox" do |v|
        v.memory = definition[:memory] unless definition[:memory].nil?
        v.cpus = definition[:cpus] unless definition[:cpus].nil?
      end

      # == Setup port forwarding if configugred
      if not definition[:ports].nil?
        definition[:ports].each do |port_info|
          c.vm.network :forwarded_port, port_info
        end
      end

      c.vm.provision "ansible_local" do |ansible|
        ansible.verbose = "vvv"
        ansible.playbook = "site.yml"
        ansible.become = true
        ansible.groups = groups
      end

    end
  end
end
