################################################################
#
# Author: 	Mark Higginbottom
#
# Date:		27/09/2016
#
#Vagrant PROJECT file to create Ansible provisioned VM for Ubuntu Python Ansible
#
#
#################################################################
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'


Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  ##upython VM
  config.vm.define "upython" do |upython|
#    upython.vm.box = "bento/centos-7.2"
#    upython.vm.box = "pbarriscale/centos7-gui"
    upython.vm.box = "ubuntu/trusty64"
    upython.vm.hostname = 'upython'

    upython.vm.network :private_network, ip: "192.168.100.201"
    upython.vm.network :forwarded_port, guest: 22, host: 20122, id: "ssh"
    upython.vm.network :forwarded_port, guest: 7272, host: 27272, id: "upythontestport"


    upython.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 2018]
      v.customize ["modifyvm", :id, "--name", "upython"]
# Show VM?
#	  v.gui = true
    end

	#map shared directory
	upython.vm.synced_folder "shared", "/home/vagrant/shared"

	upython.vm.provision "shell", inline: <<-SHELL
		#Basic vanilla installation to bootstrap Ansible.
		#set ownership of shared directory to vagrant
		sudo chown -R vagrant:vagrant /home/vagrant/shared
		##Install Ansible on upython node
		sudo apt-get update -y
		sudo apt-get install vim -y
		sudo apt-get install software-properties-common
        sudo apt-add-repository ppa:ansible/ansible
        sudo apt-get install ansible
		##Install Git
		sudo apt-get install git -y
		##Turn off ansible key checking
		export ANSIBLE_HOST_KEY_CHECKING=false
	SHELL

	##run info script
    upython.vm.provision "info", type: "shell", path: "scripts/vminfo.sh"

	##Ansible provisioning from playbook that is on the Guest VM
	upython.vm.provision "ansible_local" do |ansible|
		ansible.verbose = "true"
		ansible.extra_vars = {servers: "upython"} #inject the name of the server we want to apply this ansible config to.
		ansible.playbook = "shared/ansible/python-test1/site.yml"
    end

  end

end
