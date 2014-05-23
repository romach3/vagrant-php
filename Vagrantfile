# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.define :romach3 do |romach3_config|
        romach3_config.vm.box = "precise32"
        romach3_config.vm.box_url = "http://files.vagrantup.com/precise32.box"
        romach3_config.ssh.forward_agent = true

        # This will give the machine a static IP uncomment to enable
        romach3_config.vm.network :private_network, ip: "192.168.200.101"
        #romach3_config.vm.network :forwarded_port, guest: 80, host: 80, auto_correct: true
        #romach3_config.vm.network :forwarded_port, guest: 3306, host: 8889, auto_correct: true
        #romach3_config.vm.network :forwarded_port, guest: 5432, host: 5433, auto_correct: true

        romach3_config.vm.hostname = "romach3"
        romach3_config.vm.synced_folder "www", "/var/www", {:mount_options => ['dmode=777','fmode=777']}
        romach3_config.vm.provision :shell, :inline => "echo \"Europe/London\" | sudo tee /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata"

        romach3_config.vm.provider :virtualbox do |v|
            v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
            v.customize ["modifyvm", :id, "--memory", "512"]
        end

        romach3_config.vm.provision :puppet do |puppet|
            puppet.manifests_path = "puppet/manifests"
            puppet.manifest_file  = "phpbase.pp"
            puppet.module_path = "puppet/modules"
            #puppet.options = "--verbose --debug"
        end

        romach3_config.vm.provision :shell, :path => "puppet/scripts/enable_remote_mysql_access.sh"
    end
end