# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	# All Vagrant configuration is done here. The most common configuration
	# options are documented and commented below. For a complete reference,
	# please see the online documentation at vagrantup.com.

	# Every Vagrant virtual environment requires a box to build off of.
	config.vm.box = config.user.box.name

	# The url from where the 'config.vm.box' box will be fetched if it
	# doesn't already exist on the user's system.
	config.vm.box_url = config.user.box.url
	
	# The hostname of the computer
	config.vm.hostname = "vagrant"

	# Create a private network, which allows host-only access to the machine
	# using a specific IP.
	config.vm.network :private_network, ip: "10.11.12.13"

	# Hostmanager plugin settings
	config.hostmanager.enabled = true
	config.hostmanager.manage_host = true
	config.hostmanager.aliases = %w(v.l vagrant.l)

	# Increase system's memory
	config.vm.provider "virtualbox" do |v|
		v.memory = 1024
	end

	# Share an additional folder to the guest VM. The first argument is
	# the path on the host to the actual folder. The second argument is
	# the path on the guest to mount the folder. And the optional third
	# argument is a set of non-required options.
	config.vm.synced_folder config.user.work.folder, "/vagrant-nfs", :nfs => true # nfs should be ignored on Windows
	
	config.bindfs.bind_folder "/vagrant-nfs", "/www",
		:owner => "www-data",
		:group => "www-data",
		:perms => "777"

	# Simple bash script to check if Chef is installed
	config.vm.provision "shell", path: "bash/bootstrap.sh"

	# After Chef is installed, let Chef do the magic
	config.vm.provision :chef_solo do |chef|
		chef.roles_path = "chef/roles/"
		chef.cookbooks_path = ["chef/cookbooks/", "chef/berks-cookbooks/"]
		chef.add_role "development"
	end
end
