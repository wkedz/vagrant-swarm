# -*-: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
    config.vm.box = "ubuntu/focal64"
    
    (1..3).each do |nr|
      config.vm.define "mng#{nr}" do |node|
        node.vm.network 'private_network', ip: "192.168.100.10#{nr}"
        node.vm.network 'forwarded_port', id: 'ssh', host: "810#{nr}", guest: "22"
        node.vm.hostname = "mng#{nr}"

        if nr == 1
          node.vm.provision 'shell', path: 'scripts/swarm-init-cluster.sh'
        else
          node.vm.provision 'shell', path: 'scripts/swarm-join-manager.sh'
        end
      end
    end

    (1..2).each do |nr|
      config.vm.define "wrk#{nr}" do |node|
        node.vm.network 'private_network', ip: "192.168.100.11#{nr}"
        node.vm.network 'forwarded_port', id: 'ssh', host: "811#{nr}", guest: "22"
        node.vm.hostname = "wrk#{nr}"

        node.vm.provision 'shell', path: 'scripts/swarm-join-worker.sh'
      end
    end

    config.vm.provider 'virtualbox' do |vb|
      vb.memory = '2048'
      vb.cpus = 1 
    end

    config.vm.provision 'docker'
end
