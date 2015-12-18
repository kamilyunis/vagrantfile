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


  config.vm.define :salt do |salt|
    salt.vm.box = "anchorbox"
    salt.vm.host_name = 'salt'
    salt.vm.network "private_network", ip: "192.168.50.10"
    salt.vm.synced_folder "salt/salt/", "/srv/salt"
    salt.vm.synced_folder "salt/pillar/", "/srv/pillar"
   
   salt.vm.provision :salt do |salt|
       salt.master_config = "salt/salt/etc/master"
       salt.minion_config = "salt/salt/etc/minion"
       salt.master_key = "salt/keys/minion.pem"
       salt.master_pub = "salt/keys/minion.pub"
       salt.minion_key = "salt/keys/minion.pem"
       salt.minion_pub = "salt/keys/minion.pub"
       salt.seed_master = {
                         "salt" => "salt/keys/minion.pub",
#                          "minion2" => "salt/keys/minion.pub"
                         }

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
    web_config.vm.network "private_network", ip: "192.168.50.15"
    web_config.vm.network "forwarded_port", guest: 80, host: 8080
#    web_config.vm.synced_folder "salt/salt/", "/srv/salt"
#  end
 
  web_config.vm.provision :salt do |salt|
        salt.minion_config = "salt/salt/etc/minion_web"
#	salt.run_highstate = true
        salt.verbose = true
	salt.colorize = true
        salt.log_level = "info"
#	salt.bootstrap_script = "install_salt.sh"
#       salt.run_highstate = true
#       salt.install_type = "daily"

  end
 end


  config.vm.define :monitoring do |monitoring|
    monitoring.vm.box = "anchorbox"
    monitoring.vm.host_name = 'monitoring'
    monitoring.vm.network "private_network", ip: "192.168.50.20"
    monitoring.vm.network "forwarded_port", guest: 2812, host: 8082
    monitoring.vm.network "forwarded_port", guest: 80, host: 8081
#    monitoring.vm.synced_folder "salt/salt/", "/srv/salt"
#    monitoring.vm.synced_folder "salt/salt/", "/srv/salt"
   
  monitoring.vm.provision :salt do |salt|
       salt.minion_config = "salt/salt/etc/minion_monitoring"
#       salt.run_highstate = true
#      salt.install_type = "stable"
       salt.verbose = true
       salt.colorize = true
       salt.log_level = "info"
       salt.colorize = true
#      salt.bootstrap_options = "-P -c /tmp"
  end
  end
end
