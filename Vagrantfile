# -*- mode: ruby -*-
# vi: set ft=ruby :

hostname = 'skynet'
server_ip = "10.10.10.10"
server_cpus = "2"
server_memory = "1024"
server_swap = "2048"
server_timezone = "UTC"

Vagrant.configure(2) do |config|
    config.vm.box = "v0rtex/xenial64"
    config.vm.network :private_network, ip: server_ip
    config.vm.provision "docker"
    config.vm.hostname = hostname
    config.ssh.forward_agent = true
    config.vm.synced_folder "./src", "/vagrant",
        id: "core",
        :nfs => true,
        :mount_options => ['nolock,vers=3,udp,noatime,actimeo=2,fsc']
    config.vm.provider :virtualbox do |vb|
        vb.name = hostname
        vb.customize ["modifyvm", :id, "--cpus", server_cpus]
        vb.customize ["modifyvm", :id, "--memory", server_memory]
        vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
    end
    config.vm.provision "shell", inline: "locale-gen", privileged: true
    config.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y fish
        chsh -s /usr/bin/fish vagrant
    SHELL

    # Global Gitignore
    config.vm.provision "file", source: "./files/.gitignore", destination: "~/.gitignore"
    config.vm.provision "file", source: "./files/.gitconfig", destination: "~/.gitconfig"
end
