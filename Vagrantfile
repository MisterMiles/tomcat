# -*- mode: ruby -*-
# # vi: set ft=ruby :VAGRANTFILE_API_VERSION = "2"

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider :libvirt do |domain|
    domain.memory = 2048
    domain.cpu_mode = "host-model"
    domain.nested = true
    domain.random :model => 'random' # Passthrough /dev/random
  end

  config.vm.box = "centos/7"
  config.vm.box_download_checksum = "10907f19d5ff7d5bab5bef414bdb7305bbff39502001bd36b82ef3a9afc62910"
  config.vm.box_download_checksum_type = "sha256"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  #hostnames = ['router','desktop','laptop','nas','htpc']
  hostnames = ['tomcat']
  hostnames.each do |name|
  config.vm.define "#{name}" do |system|
    system.vm.host_name = "#{name}"
    system.vm.provision "ansible" do |ansible|
        ansible.playbook = "#{name}.yml"
        ansible.inventory_path = "inventories/vagrant"
        ansible.limit = "all" # run ansible in parallel for all machines
        ansible.verbose = "vv"
      end
    end
  end

  config.vm.define :tomcat do |tomcat|
    tomcat.vm.hostname = "tomcat.dev"
    tomcat.vm.network :private_network,
                      :libvirt__network_name => 'default',
                      :libvirt__dhcp_enabled => false,
                      :ip => "192.168.122.10"
  end
end
