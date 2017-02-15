# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.box = "debian/contrib-jessie64"
  config.vm.network "public_network", 
    use_dhcp_assigned_default_route: true

  config.vm.provider "virtualbox" do |vb|
    vb.name = "devops-station"
    vb.gui = true
    vb.cpus = 2
    vb.memory = "1024"
    
    vb.customize ["modifyvm", :id, "--pae", "on"]
    vb.customize ["modifyvm", :id, "--audio", "none"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--mouse", "usbtablet"]
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--largepages", "on"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
  end
  
  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND='noninteractive'
    sudo apt-get update -yqq > /dev/null
    sudo apt-get dist-upgrade -yqq
  SHELL

  config.vm.provision "ansible_local" do |ansible|
    ansible.install           = true
    ansible.install_mode      = "pip"
    ansible.inventory_path    = "hosts"
    ansible.galaxy_role_file  = "requirements.yml"
    ansible.limit             = "all"
    ansible.playbook          = "playbook.yml"
  end

  config.vm.provision "shell", run: "always", env: { "DEBIAN_FRONTEND" => "noninteractive" }, inline: <<-SHELL
      echo "Addresse IP: $(ip address show dev eth1 | grep -E 'inet\s+' | awk '{ print $2 }')"
  SHELL

end
