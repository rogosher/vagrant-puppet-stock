# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # Default box is a virtualbox box
  config.vm.box = "ubuntu/trusy64"

  config.ssh.forward_agent = true
  config.vm.hostname = "dev.rails.box"

  # Folder contains private data for applications
  #config.vm.synced_folder "../vagrant_private",
  #  "/tmp/vagrant-private", type: "nfs"

  # Use static IP for NFS
  config.vm.network :private_network, ip: "10.0.1.20"

  # Provide a private key for connecting to private git servers
  config.vm.provision :file, source: "~/.ssh/vagrant_keys/vagrant_rsa",
    destination: ".ssh/vagrant_rsa"

  config.vm.provider :virtualbox do |vbox, override|
    override.vm.box = "ubuntu/trusty64"
    vbox.memory = 4096
    vbox.cpus = 2
  end

  config.vm.provider :libvirt do |libvirt, override|
    override.vm.box = "matjazp/ubuntu-trusty64"
    libvirt.management_network_name = "vagrant-libvirt-new-one"
    libvirt.management_network_address = "192.168.124.0/24"
    libvirt.memory = 5120
    libvirt.cpus = 2
  end

  # share a directory for :datadir, defined in hiera.yaml
  config.vm.synced_folder "puppet/hieradata",
    "/tmp/vagrant-puppet/hieradata", type: "nfs"

  # r10k configuration
  config.r10k.puppet_dir = 'puppet'
  config.r10k.puppetfile_path = 'puppet/Puppetfile'

  # provision with puppet
  config.vm.provision :puppet do |puppet|
    if ENV.key?('PUPPET_OPTS')
      puppet.options = ENV['PUPPET_OPTS'].split(' ')
    end
    puppet.module_path = 'puppet/modules'
    puppet.manifests_path = 'puppet/manifests'
    puppet.manifest_file = 'site.pp'
    puppet.hiera_config_path = 'hiera.yaml'
    puppet.working_directory = '/tmp/vagrant-puppet'
    puppet.synced_folder_type = 'nfs'
    puppet.synced_folder_args = ['-a', '--delete', '--exclude=fixtures']
  end
end
