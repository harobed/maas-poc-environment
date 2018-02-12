  # -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define :maas do |maas|

    # Use Ubuntu LTS version
    maas.vm.box = "ubuntu/xenial64"
    maas.vm.hostname = "pxe-server"
    maas.vm.synced_folder '.', '/vagrant', disabled: true
    maas.vm.network "private_network", ip: "192.168.0.254", virtualbox__intnet: "pxe_network"
    maas.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true
    maas.vm.network "forwarded_port", guest: 8000, host: 8000, auto_correct: true
    maas.ssh.forward_agent = true

    maas.vm.provider :virtualbox do |vb|
      vb.memory = '1024'
      vb.cpus = '1'
    end
  end

  config.vm.define :blank_server do |blank_server|
    ip = "192.168.0.11"

    blank_server.vm.box = "TimGesekus/pxe-boot"
    blank_server.vm.network "private_network", :adapter=>1, ip: ip, :mac => "0800278E158A" , auto_config: false, virtualbox__intnet: "pxe_network"
    blank_server.vm.synced_folder '.', '/vagrant', disabled: true
    blank_server.ssh.host = ip
    blank_server.ssh.username = 'root'
    blank_server.vm.provider "virtualbox" do |vb, override|
        vb.gui = true
        # Chipset needs to be piix3, otherwise the machine wont boot properly.
        vb.customize [
          "modifyvm", :id,
          "--pae", "off",
          "--chipset", "piix3",
          '--boot1', 'net',
          '--boot2', 'disk',
          '--boot3', 'none',
          '--boot4', 'none'
        ]
    end
  end
end
