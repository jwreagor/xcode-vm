# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "osx1011"
  config.vm.box_check_update = false
  config.vm.network "public_network"
  config.vm.provider :virtualbox

  config.vm.synced_folder ".", "/vagrant", type: "rsync"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "2048"
  end

  # config.vm.provision "shell", inline: <<-SHELL
  #   /usr/bin/xcode-select --install
  #   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  #   sudo gem install fastlane
  #   gem cleanup
  # SHELL
end
