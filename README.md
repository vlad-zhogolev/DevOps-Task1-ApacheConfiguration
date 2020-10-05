Static page can be accessed via: [www.zv-my-site.com](www.zv-my-site.com).

Pythonic page can be accessed via: [www.zv-python-time.com](www.zv-python-time.com)

Use command `vagrant box add zvladyuha/zv-apache` to add box.

The following `Vagrantfile`can be used for downloaded box:
```
Vagrant.configure("2") do |config|
  config.vm.box = "zvladyuha/zv-apache"
  config.vm.box_version = "1.0.0"
  config.vagrant.plugins = "vagrant-hostsupdater"
  config.hostsupdater.aliases = ["www.zv-my-site.com", "www.zv-python-time.com"]
  config.vm.network "private_network", ip: "192.168.50.11"
  config.vm.network "forwarded_port", guest: 80, host: 8893
end
```
