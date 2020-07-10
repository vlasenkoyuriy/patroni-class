# -*- mode: ruby -*-
# vi: set ft=ruby :

$provision_common = <<SCRIPT
/vagrant/install
SCRIPT

Vagrant.configure("2") do |config|

  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false

  boxes = [
    { :name => "cluster1-h1", :hostname => "cluster1-h1", :ip => "192.168.11.110" },
    { :name => "cluster1-h2", :hostname => "cluster1-h2", :ip => "192.168.11.120" },
    { :name => "cluster1-h3", :hostname => "cluster1-h3", :ip => "192.168.11.130" }
  ]
  
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.box = "centos/7"
      config.vm.hostname = opts[:hostname]
      config.vm.network "private_network", ip: opts[:ip]
      config.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.name = opts[:name]
        vb.cpus = 2
    end

    if opts[:name] == boxes.last[:name]
      config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/consul.yml"
        ansible.inventory_path = "ansible/inventory"
        ansible.verbose = "v"
        ansible.limit = "all"
        ansible.become = true
      end
    end  

  end

  config.vm.provision "shell", inline: $provision_common
  
  end

end
