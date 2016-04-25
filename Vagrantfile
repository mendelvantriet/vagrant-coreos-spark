# -*- mode: ruby -*-
# # vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

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

  config.vm.box = "centos/7"

  config.ssh.insert_key = false
  
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

#  config.vm.provision :shell, path: "bootstrap.sh"
  
end
