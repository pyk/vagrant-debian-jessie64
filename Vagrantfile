# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.define "jessie" do |jessie|
    jessie.vm.box = "debian/jessie64"
    jessie.vm.hostname = "jessie"
    jessie.vm.network "private_network", ip: "192.168.33.10"

    jessie.vm.provider :virtualbox do |vb|
      vb.gui = false
      vb.memory = 1024
      vb.cpus = 2
    end

  end
end
