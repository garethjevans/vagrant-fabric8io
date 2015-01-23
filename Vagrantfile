# -*- mode: ruby -*-
# vi: set ft=ruby :

# Check and Install required plugins if missing
installed_plugins = false
required_plugins=%w( vagrant-reload )
required_plugins.each do |plugin|
  if !Vagrant.has_plugin?plugin
    system "vagrant plugin install #{plugin}"
    installed_plugins = true
  end
end

if installed_plugins
  puts "Please re-run 'vagrant up' command"
  exit
end

Vagrant.configure("2") do |config|
	
	config.vm.box = "hansode/fedora-21-server-x86_64"
	
	config.vm.provider "virtualbox" do |v|
    	v.name = "fabric8io-environment"
		v.gui = true
		v.customize ["modifyvm", :id, "--cpus", "4"]
      	v.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
      	v.customize ["modifyvm", :id, "--monitorcount", "1"]
      	v.customize ["modifyvm", :id, "--memory", "4096"]
      	v.customize ["modifyvm", :id, "--vram", "128"]
      	v.customize ["modifyvm", :id, "--ioapic", "on"]
      	v.customize ["modifyvm", :id, "--accelerate3d", "on"]
	end

	config.vm.provision "shell", inline: "yum upgrade -y"
	config.vm.provision "shell", inline: "yum install -y java-1.7.0-openjdk"
	config.vm.provision "shell", inline: "yum remove -y docker"
	config.vm.provision "shell", inline: "yum install -y docker-io"
	config.vm.provision "shell", inline: "usermod -a -G docker vagrant"
	config.vm.provision "shell", inline: "service docker restart"
	config.vm.provision "shell", inline: "bash <(curl -sSL https://bit.ly/get-fabric8)"
	config.vm.provision :reload
end
