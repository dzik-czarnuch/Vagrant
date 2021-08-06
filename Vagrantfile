Vagrant.configure("2") do |config|

  nodes = [
      {
          :hostname => "node1",
          :box => "generic/centos8",
          :ip => "192.168.10.10",
          :mem => "256",
          :cpu => "1"
      },
      {
          :hostname => "node2",
          :box => "generic/centos8",
          :ip => "192.168.10.11",
          :mem => "512",
          :cpu => "2"
      },
      {
          :hostname => "node3",
          :box => "generic/centos8",
          :ip => "192.168.10.12",
          :mem => "512",
          :cpu => "2"
      }
  ]

  nodes.each do |machine|
      config.vm.define machine[:hostname] do |node|
          node.vm.box = machine[:box]
          node.vm.hostname = machine[:hostname]
          node.vm.network :private_network, ip: machine[:ip]
          node.vm.provider :virtualbox do |vb|
              vb.customize ["modifyvm", :id, "--name", machine[:hostname]]
              vb.customize ["modifyvm", :id, "--memory", machine[:mem]]
              vb.customize ["modifyvm", :id, "--vram", 128]
              vb.customize ["modifyvm", :id, "--cpus", machine[:cpu]]
          end
          config.vm.synced_folder "/mnt/vagrant", "/vagrant"
          node.vm.provision "ansible_local" do |ansible|
            ansible.become = yes
            ansible.playbook = "main.yaml"
          end
      end
  end
end
