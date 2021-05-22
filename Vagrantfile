# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
    config.env.enable
    config.ssh.insert_key = false    
    # VM's
    config.vm.define :enki do |enki|
	enki.vm.box = "focal-server-cloudimg-amd64-vagrant"
        enki.vm.box_url = "https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64-vagrant.box"
        enki.vm.hostname = "enki"
        enki.vm.network :private_network, virtualbox__intnet: "privateSubnet",
            name: "eth4",
            ip: "192.168.14.20",
            netmask: "255.255.255.0"
        enki.vm.network :public_network, bridge: "en1: Wi-Fi (AirPort)",
            name: "eth5",
            ip: "192.168.15.20",
            netmask: "255.255.255.0"
        enki.vm.network :public_network, bridge: "en1: Wi-Fi (AirPort)",
            name: "eth6",
            ip: "192.168.15.30",
            netmask: "255.255.255.0"
        enki.vm.cloud_init do |cloud_init|
            cloud_init.content_type = "text/cloud-config"
            cloud_init.path = "./cloud-init/UserData.yaml"
        end
        enki.vm.provider :virtualbox do |v|
            v.memory = 2048
            v.cpus = 2
        end
    end
    config.vm.define :enlil do |enlil|
	enlil.vm.box = "focal-server-cloudimg-amd64-vagrant"
        enlil.vm.box_url = "https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64-vagrant.box"
        enlil.vm.hostname = "enlil"
        enlil.vm.network :private_network, virtualbox__intnet: "privateSubnet",
            name: "enp0s2",
            ip: "192.168.14.21",
            netmask: "255.255.255.0"
        enlil.vm.network :public_network, bridge: "en1: Wi-Fi (AirPort)",
            name: "enp0s3",
            ip: "192.168.15.21",
            netmask: "255.255.255.0"
        enlil.vm.network :public_network, bridge: "en1: Wi-Fi (AirPort)",  
            name: "enp0s4",
            ip: "192.168.15.31",
            netmask: "255.255.255.0"
        enlil.vm.cloud_init do |cloud_init|
            cloud_init.content_type = "text/cloud-config"
            cloud_init.path = "./cloud-init/UserData.yaml"
        end
        enlil.vm.provider :virtualbox do |v|
            v.memory = 2048
            v.cpus = 2
        end
    end
    config.vm.define :utu do |utu|
        utu.vm.box = "focal-server-cloudimg-amd64-vagrant"
        utu.vm.box_url = "https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64-vagrant.box"
        utu.vm.hostname = "utu"
        utu.vm.network :private_network, virtualbox__intnet: "privateSubnet",
            name: "enp0s2",
            ip: "192.168.14.22",
            netmask: "255.255.255.0"
        utu.vm.network :public_network, bridge: "en1: Wi-Fi (AirPort)",
            name: "enp0s3",
            ip: "192.168.15.22",
            netmask: "255.255.255.0"
        utu.vm.network :public_network, bridge: "en1: Wi-Fi (AirPort)",
            name: "enp0s4",
            ip: "192.168.15.32",
            netmask: "255.255.255.0"
        utu.vm.cloud_init do |cloud_init|
            cloud_init.content_type = "text/cloud-config"
            cloud_init.path = "./cloud-init/UserData.yaml"
        end
        utu.vm.provider :virtualbox do |v|
            v.memory = 2048
            v.cpus = 2
        end
        utu.vm.provision "master-playbook", type: 'ansible' do |ansible|
            ansible.playbook = "provisioning/master-playbook.yaml"
            ansible.config_file = "ansible.cfg"
            ansible.inventory_path = "provisioning/hosts"
            ansible.limit = "all"
            ansible.extra_vars = {
               node_ip: "192.168.15.20",
            }
        end
        utu.vm.provision "worker-playbook", type: 'ansible' do |ansible|
            ansible.playbook = "provisioning/nodes-playbook.yaml"
            ansible.config_file = "ansible.cfg"
            ansible.inventory_path = "provisioning/hosts"
            ansible.limit = "all"
        end
        utu.vm.provision "deploy-charts", type: 'ansible' do |ansible|
            ansible.playbook = "provisioning/deploy-helm-charts-playbook.yaml"
            ansible.limit = "all"
            ansible.config_file = "ansible.cfg"
            ansible.inventory_path = "provisioning/hosts"
        end
    end 
end
