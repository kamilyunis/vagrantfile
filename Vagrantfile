# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
   config.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--usb", "on"]
     vb.customize ["modifyvm", :id, "--usbehci", "off"]
     vb.memory = "256"
     vb.cpus = "1"
   end

#   config.vm.define :default do |default|
#    default.vm.box = "itrushchenko/CentOS_6.6"
#    default.vm.host_name = 'default'
#    default.vm.network "private_network", ip: "192.168.50.10"
#   end


  config.vm.define :salt do |salt|
    salt.vm.box = "anchorbox"
    salt.ssh.password = "root"
    salt.ssh.password = "vagrant"
    salt.vm.host_name = 'salt'
    salt.vm.network "private_network", ip: "192.168.50.10"
    salt.vm.synced_folder "salt/salt/", "/srv/salt"
    salt.vm.synced_folder "salt/pillar/", "/srv/pillar"
   
   salt.vm.provision :salt do |salt|
       salt.minion_config = "salt/etc/minion"
       salt.run_highstate = true
       salt.verbose = true

#	salt.bootstrap_script = "install_salt.sh"
#       salt.run_highstate = true	
#       salt.install_type = "daily"
    end
  end

  config.vm.define :web do |web_config|
    web_config.vm.box = "anchorbox"
    web_config.vm.host_name = 'web'
    web_config.vm.network "private_network", ip: "192.168.50.11"
    web_config.vm.network "forwarded_port", guest: 80, host: 8080
    web_config.vm.synced_folder "salt/salt/", "/srv/salt"
#  end
 
  web_config.vm.provision :salt do |salt|
        salt.minion_config = "salt/etc/minion_web"
        salt.run_highstate = true
        salt.verbose = true
#	salt.bootstrap_script = "install_salt.sh"
#       salt.run_highstate = true
#       salt.install_type = "daily"

  end
 end




  config.vm.define :monitoring do |monitoring|
    monitoring.vm.box = "anchorbox"
    monitoring.vm.host_name = 'monitoring'
    monitoring.vm.network "private_network", ip: "192.168.50.12"
  end
end
