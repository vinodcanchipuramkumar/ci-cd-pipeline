Vagrant.configure(2) do | config |
    config.vm.define "ansible_controlnode" do | acn |
        acn.vm.box = "techsriman/ubuntu-22.04"
        acn.vm.disk :disk, size: "20GB" 
        acn.vm.network "private_network", ip: "192.168.10.4", virtualbox_intnet: "ansiblenetwork"
        acn.vm.provider "virtualbox" do | vb |
            vb.name="ansible_controlnode"
            vb.cpus=2
            vb.memory=2048         
            vb.gui=true   
        end
        acn.vm.provision "file", source: "keys/ansible", destination: "/home/vagrant/.ssh/"
        acn.vm.provision "shell", path: "sh/setupcontrolnodeserver.sh"
    end
    (1..4).each do | index |
        config.vm.define "ansible_managednode_#{index}" do | node |
            node.vm.box = "ubuntu/focal64"
            node.vm.network "private_network", ip: "192.168.10.#{index+4}", virtualbox_intnet: "ansiblenetwork"
            node.vm.provider "virtualbox" do | vb |
                vb.name="ansible_managednode_#{index}"
                vb.cpus=2
                vb.memory=2048
            end
            node.vm.provision "shell", path: "sh/setupmanagednode.sh"
        end
    end
end