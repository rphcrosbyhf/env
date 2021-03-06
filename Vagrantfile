# -*- mode: ruby -*-
# vi: set ft=ruby :

hostname = 'skynet'
server_ip = "10.10.10.10"
server_cpus = "2"
server_memory = "2048"
server_swap = "2048"
server_timezone = "UTC"

Vagrant.configure(2) do |config|
    config.vm.box = "xenji/ubuntu-17.04-server"
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
        add-apt-repository ppa:ondrej/php
        apt-get update
        apt-get install -y fish php7.1 php-xdebug php7.1-xml zsh gnupg rng-tools

        # Install PHPCS and PHP Mess Detector
        wget -c http://static.phpmd.org/php/latest/phpmd.phar -O /usr/local/bin/phpmd && chmod +x /usr/local/bin/phpmd
        wget -c https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -O /usr/local/bin/phpcs && chmod +x /usr/local/bin/phpcs

        # Install composer
        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
        php composer-setup.php
        php -r "unlink('composer-setup.php');"
        mv composer.phar /usr/local/bin/composer

        # Install composer
        curl -o /usr/local/bin/docker-compose -L "https://github.com/docker/compose/releases/download/1.11.2/docker-compose-$(uname -s)-$(uname -m)"
        chmod +x /usr/local/bin/docker-compose

        # Install oh-my-zsh
        git clone git://github.com/robbyrussell/oh-my-zsh.git /home/vagrant/.oh-my-zsh
        cp /home/vagrant/.oh-my-zsh/templates/zshrc.zsh-template /home/vagrant/.zshrc
        chsh -s /usr/bin/zsh vagrant
    SHELL

    # Global Gitignore
    config.vm.provision "file", source: "./files/.gitignore", destination: "~/.gitignore"
    config.vm.provision "file", source: "./files/.gitconfig", destination: "~/.gitconfig"
end
