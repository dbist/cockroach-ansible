# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

 # Hostmanager specific configs
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.

  config.vm.box = "ubuntu/focal64"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled:true

  # Update virtual guest additions
  config.vbguest.auto_update = false

  # Provision 2GB and 2 vcpu for each machine as per recommended
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2024
    vb.cpus = 2
    vb.linked_clone = true

    # WIP, this approach would not need vagrant-persistent-storage plugin
    #file_to_disk = './large_disk.vdi'
    #  unless File.exist?(file_to_disk)
    #    vb.customize ['createhd', '--filename', file_to_disk, '--size', 10 * 1024]
    #  end
    #  vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]

    # Provision a second disk
    config.persistent_storage.enabled = true
    config.persistent_storage.size = 10000
    config.persistent_storage.mountname = 'data'
    config.persistent_storage.filesystem = 'ext4'
    config.persistent_storage.mountpoint = '/data'
    config.persistent_storage.volgroupname = 'cockroachdb'
    config.persistent_storage.partition = false
    config.persistent_storage.variant = 'Fixed'
  end

  # roach1
  config.vm.define "roach1" do |roach1|
    roach1.vm.hostname = "roach1.example.com"
    roach1.vm.network :private_network, ip: "192.168.60.4"
    roach1.hostmanager.aliases = %w(roach1.localdomain roach1.example.com)
    roach1.persistent_storage.location = "./roach1.vdi"
  end

  # roach2
  config.vm.define "roach2" do |roach2|
    roach2.vm.hostname = "roach2.example.com"
    roach2.vm.network :private_network, ip: "192.168.60.5"
    roach2.hostmanager.aliases = %w(roach2.localdomain roach2.example.com)
    roach2.persistent_storage.location = "./roach2.vdi"
  end

  # roach3
  config.vm.define "roach3" do |roach3|
    roach3.vm.hostname = "roach3.example.com"
    roach3.vm.network :private_network, ip: "192.168.60.6"
    roach3.hostmanager.aliases = %w(roach3.localdomain roach3.example.com)
    roach3.persistent_storage.location = "./roach3.vdi"
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Provisioning configuration for Ansible.
  config.vm.provision "ansible" do |ansible|
  # ansible.playbook = "roles/aervits.cockroachdb/playbook.yml"
    ansible.playbook = "cockroachdb-playbook.yml"
    ansible.compatibility_mode = "2.0"
  end

end
