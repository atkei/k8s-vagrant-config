# -*- mode: ruby -*-
# vi:set ft=ruby sw=2 ts=2 sts=2:

require 'yaml'
conf = YAML.load_file 'config.yaml'

IP_NW = conf['ip_nw']
SSH_PORT_START = conf['ssh_port_start']
MASTER_IP_START = conf['master']['ip_start']
WORKER_IP_START = conf['worker']['ip_start']
LB_IP_START = conf['lb']['ip_start']

Vagrant.configure("2") do |config|
  config.vm.box = conf['os']
  config.vm.box_check_update = false

  config.ssh.insert_key = false
  config.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "~/.ssh/id_rsa"
  config.vm.provision "file", source: "config.yaml", destination: "config.yaml"
  config.vm.provision "shell", path: "bootstrap.sh"

  (1..conf['master']['num_nodes']).each do |i|
    config.vm.define "master-#{i}" do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.name = "master-#{i}"
        vb.cpus = conf['master']['cpus']
        vb.memory = conf['master']['memory']
      end
      node.vm.hostname = "master-#{i}"
      node.vm.network "private_network", ip: IP_NW + "#{MASTER_IP_START + i - 1}"
      node.vm.network "forwarded_port", guest: 22, host: "#{SSH_PORT_START + MASTER_IP_START + i - 1}"
    end
  end

  (1..conf['worker']['num_nodes']).each do |i|
    config.vm.define "worker-#{i}" do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.name = "worker-#{i}"
        vb.cpus = conf['worker']['cpus']
        vb.memory = conf['worker']['memory']
      end
      node.vm.hostname = "worker-#{i}"
      node.vm.network "private_network", ip: IP_NW + "#{WORKER_IP_START + i - 1}"
      node.vm.network "forwarded_port", guest: 22, host: "#{SSH_PORT_START + WORKER_IP_START + i - 1}"
    end
  end

  (1..conf['lb']['num_nodes']).each do |i|
    config.vm.define "lb-#{i}" do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.name = "lb"
        vb.cpus = conf['lb']['cpus']
        vb.memory = conf['lb']['memory']
      end
      node.vm.hostname = "lb-#{i}"
      node.vm.network :private_network, ip: IP_NW + "#{LB_IP_START}"
      node.vm.network "forwarded_port", guest: 22, host: "#{SSH_PORT_START + LB_IP_START + i - 1}"
    end
  end
end
