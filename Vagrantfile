# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  config.vagrant.plugins = "vagrant-hostsupdater"
  config.hostsupdater.aliases = ["www.zv-my-site.com", "www.zv-python-time.com"]
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "private_network", ip: "192.168.50.11"
  config.vm.network "forwarded_port", guest: 80, host: 8893

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
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.

  config.vm.provision "file", source: "apache/html", destination: "html"
  config.vm.provision "file", source: "apache/scripts", destination: "scripts"
  config.vm.provision "file", source: "apache/sites-available", destination: "sites-available"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
    apt install -y python3-pip
    apt-get install -y apache2-dev
    apt-get install -y libapache2-mod-wsgi

    mkdir -p /var/www/hello_world
    cp -r html/index.html /var/www/hello_world/index.html
    cp -r sites-available/hello-world.conf /etc/apache2/sites-available/hello-world.conf
    cp -r sites-available/python-time.conf /etc/apache2/sites-available/python-time.conf

    #mkdir -p /var/www/python_time
    # cp -r scripts/get_time.py /usr/lib/cgi-bin/get_time.py
    mkdir -p /usr/local/www/wsgi-scripts/
    cp -r scripts/get_time.wsgi /usr/local/www/wsgi-scripts/get_time.wsgi
    chmod a+x /usr/local/www/wsgi-scripts/get_time.wsgi

    a2enmod wsgi
    a2ensite hello-world.conf
    a2ensite python-time.conf
    service apache2 restart
  SHELL
end
