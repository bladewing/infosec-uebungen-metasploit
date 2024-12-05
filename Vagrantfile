# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.define "ub1404" do |ub1404|
    ub1404.vm.box = "rapid7/metasploitable3-ub1404"
    ub1404.vm.hostname = "metasploitable3-ub1404"
    ub1404.ssh.username = 'vagrant'
    ub1404.ssh.password = 'vagrant'

    ub1404.vm.network "private_network", ip: '192.168.56.42', virtualbox__intnet: "internal"

    ub1404.vm.provider "virtualbox" do |v|
      v.name = "Metasploitable3-ub1404"
      v.memory = 2048
    end
  end

  config.vm.define "eve" do |eve|
    eve.vm.box = "kalilinux/rolling"
    eve.vm.hostname = "eve"
    eve.vm.network "private_network", ip: "192.168.56.41", virtualbox__intnet: "internal"
    eve.vm.provider "virtualbox" do |vb|
      vb.gui = false
    end

  end

end
