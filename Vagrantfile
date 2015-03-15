# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(2) do |config|

  config.vm.define "main", primary: true do |main|
    main.vm.hostname = "main"
    main.vm.box = "ubuntu/trusty64"
    main.vm.network "private_network", ip: "172.16.0.254", virtualbox__intnet: true
    main.vm.network "public_network"
    main.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
    end
  end
  
  numnodes = ENV['TDC_NUMNODES'] == nil ? 3 : ENV['TDC_NUMNODES']  
  nodenames = (1..numnodes).map do |nodenum|
    "node" << "#{nodenum}".rjust(2,"0")
  end

  nodenames.each do |nodename|
    config.vm.define nodename do |node|
      node.vm.hostname = nodename
      node.vm.box = "ubuntu/trusty64"
      node.vm.network "private_network", type: "dhcp", virtualbox__intnet: true
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
      "managers" => ["main"],
      "nodes" => nodenames
    }
    ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
    ansible.playbook = "playbooks/site.yaml"
    ansible.sudo = true
  end
end
