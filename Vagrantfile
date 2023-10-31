# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.box_check_update = false
  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.linked_clone = true
  end
  config.vbguest.auto_update = false

  config.vm.define "controller" do |ctrl|
    ctrl.vm.hostname = "controller"
    ctrl.vm.network "private_network", ip: "172.16.1.100", virtualbox__intnet: "common"
    ctrl.vm.network "forwarded_port", guest: 8080, host: 8080
    ctrl.vm.provision :hosts do |hosts|
      hosts.add_host '172.16.1.100', ['controller']
      hosts.add_host '100.64.0.1', ['node-a']
      hosts.add_host '100.64.0.2', ['node-b']
    end
    ctrl.vm.provision :ansible do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/controller.yaml"
      # ansible.verbose = "v"
    end
end

  config.vm.define "nat-a" do |nat|
    nat.vm.hostname = "nat-a"
    nat.vm.network "private_network", ip: "172.16.1.1", virtualbox__intnet: "common"
    nat.vm.network "private_network", ip: "192.168.1.1", virtualbox__intnet: "net-a"
    nat.vm.provision :hosts do |hosts|
      hosts.add_host '172.16.1.100', ['controller']
      hosts.add_host '100.64.0.1', ['node-a']
      hosts.add_host '100.64.0.2', ['node-b']
    end
    nat.vm.provision :ansible do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/nat_server.yaml"
    end
  end

  config.vm.define "node-a" do |node|
    node.vm.hostname = "node-a"
    node.vm.network "private_network", ip: "192.168.1.2", virtualbox__intnet: "net-a"
    node.vm.provision :hosts do |hosts|
      hosts.add_host '172.16.1.100', ['controller']
      hosts.add_host '100.64.0.1', ['node-a']
      hosts.add_host '100.64.0.2', ['node-b']
    end
    node.vm.provision :ansible do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/tailscale_client.yaml"
    end
    node.vm.provision :ansible do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/tailscale_connect.yaml"
    end
  end

  config.vm.define "nat-b" do |nat|
    nat.vm.hostname = "nat-b"
    nat.vm.network "private_network", ip: "172.16.1.2", virtualbox__intnet: "common"
    nat.vm.network "private_network", ip: "192.168.1.1", virtualbox__intnet: "net-b"
    nat.vm.provision :hosts do |hosts|
      hosts.add_host '172.16.1.100', ['controller']
      hosts.add_host '100.64.0.1', ['node-a']
      hosts.add_host '100.64.0.2', ['node-b']
    end
    nat.vm.provision :ansible do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/nat_server.yaml"
    end
  end

  config.vm.define "node-b" do |node|
    node.vm.hostname = "node-b"
    node.vm.network "private_network", ip: "192.168.1.2", virtualbox__intnet: "net-b"
    node.vm.provision :hosts do |hosts|
      hosts.add_host '172.16.1.100', ['controller']
      hosts.add_host '100.64.0.1', ['node-a']
      hosts.add_host '100.64.0.2', ['node-b']
    end
    node.vm.provision :ansible do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/tailscale_client.yaml"
    end
    node.vm.provision :ansible do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/tailscale_connect.yaml"
    end
  end
end
