# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.require_version ">= 2.0.0"
### configuration parameters
BOX_BASE = "adilkaraoz/xmrig_requirements"
BOX_VERSION = "0.0.1"
BOX_CPU_COUNT = 2
BOX_RAM_MB = "4096"

### worker nodes configuration(s)
WORKER_IP_PATTERN = "192.168.2.10"
WORKER_COUNT = 1
WORKER_PREFIX = "worker0"

$script = <<-SCRIPT
URL=pool.minexmr.com
port=4444
WALLET_ADDRESS=447yBgRnSpURcbKuK7dWZMEbDhBoqQQSCAJ89GFsDc5x9c8fw5FN53sJQVUjGPHj8R7hZ9g7eNk78DSXiThTGZPqDY1PBrS
echo connecting to $URL:$port with $WALLET_ADDRESS wallet address..
xmrig -o $URL:$port -u $WALLET_ADDRESS &
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = BOX_CPU_COUNT
    vb.memory = BOX_RAM_MB
  end

  (1..WORKER_COUNT).each do |i|
    config.vm.define "#{WORKER_PREFIX}#{i}" do |node|
     node.vm.box = BOX_BASE
     node.vm.box_version = BOX_VERSION
      node.vm.box_check_update = false
      node.vm.hostname = "#{WORKER_PREFIX}#{i}"
      node.vm.network "private_network", ip: "#{WORKER_IP_PATTERN}#{i}", netmask: "255.255.255.0"
      node.vm.provision "shell", inline: "echo 'cd /vagrant' >> ~/.bashrc && exit", privileged: false
      node.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", BOX_RAM_MB]
      end
      node.vm.provision "shell" do |s|
        s.inline = $script
      end
    end
  end
end
