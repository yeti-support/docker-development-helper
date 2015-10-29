# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?("vagrant-host-shell")
  print "Installing the vagrant-host-shell plugin for you.  Stand by.\n"
  system("vagrant plugin install vagrant-host-shell")
  raise "Please re-run vagrant, your error should be fixed now."
end

Vagrant.configure(2) do |config|
  # This is probably what you'll want to change.
  # First one is your path relative to the Vagrantfile on the OSX/Windows
  # Second is the base path you'll use inside docker-compose.yml
  config.vm.synced_folder ".", "/host"

  # VMware fusion sizing
  config.vm.provider "vmware_fusion" do |vmware|
    vmware.vmx_data["numcpus"] = 1
    vmware.gui = true
  end

  # VMware desktop sizing
  config.vm.provider "vmware_desktop" do |vmware|
    vmware.vmx_data["numcpus"] = 1
    vmware.gui = true
  end

  # Virtualbox sizing
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "1024"
  end

  # This is a pretty arbitrarily-chosen box.  Use your own if you like.
  # Mitchell's boot2docker image is missing guest additions for everything I tried,
  # otherwise it'd be here :'(
  config.vm.box = "puppetlabs/ubuntu-14.04-64-puppet"

  # Update below in the docker-machine commands if you change this
  config.vm.network "private_network", ip: "172.20.254.2"

  config.vm.provision :host_shell do |host_shell|
    # This thrashes if you have multiple providers
    host_shell.inline = 'bash -c "docker-machine rm vagrant >& /dev/null; docker-machine create vagrant -d generic --generic-ip-address 172.20.254.2 --generic-ssh-user vagrant --generic-ssh-key .vagrant/machines/default/*/private_key"'
  end

  config.vm.provision :host_shell do |host_shell|
    host_shell.inline = 'echo Run \'eval $(docker-machine env vagrant); docker-compose up\' to start your environment'
  end
end
