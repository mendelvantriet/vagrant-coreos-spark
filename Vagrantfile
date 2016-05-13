# -*- mode: ruby -*-
# # vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
CLOUD_CONFIG_PATH = File.join(File.dirname(__FILE__), "user-data")

num_nodes = 2
subnet = "192.168.9"
ip_range_start = 11


vm_gui = false
vm_cpus = 8
vm_memory = 2048

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.box = "coreos-stable"
  config.vm.box_url = "https://storage.googleapis.com/stable.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json"

  config.ssh.insert_key = false
  config.vm.synced_folder "synced_folder", "/home/core/share", id: "core", :mount_options => ['nolock,vers=3,udp']
  
  config.vm.provider :virtualbox do |v|
    v.check_guest_additions = false
    v.functional_vboxsf     = false
  end

  num_nodes.times do |i|
    vm_name = "%s-%02d" % ["node", i+1]
    config.vm.define vm_name, primary: i==0 do |config|
    
      config.vm.hostname = vm_name
      
      config.vm.provider :virtualbox do |vb|
        vb.gui = vm_gui
        vb.memory = vm_memory
        vb.cpus = vm_cpus
      end
      
      ip = "#{subnet}.#{ip_range_start+i}"
      config.vm.network "private_network", ip: ip
      
    end
  end

  if File.exist?(CLOUD_CONFIG_PATH)
    config.vm.provision :file, :source => "#{CLOUD_CONFIG_PATH}", :destination => "/tmp/vagrantfile-user-data"
    config.vm.provision :shell, :inline => "mv /tmp/vagrantfile-user-data /var/lib/coreos-vagrant/", :privileged => true
  end

end
