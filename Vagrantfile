# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/rockylinux-8"
  config.vm.box_version = "202508.03.0"

  config.vm.hostname = "homepage"

  config.vm.network "private_network", ip: "192.168.33.10"

  # 同期フォルダの設定
  config.vm.synced_folder "C:/Users/h-yos/dev/homepage", "/vagrant_data"

  # VirtualBox の設定
  config.vm.provider "virtualbox" do |vb|
    vb.name = "homepage"   # VirtualBox 上の VM 名
    vb.memory = 1024       # メモリ 1GB
    vb.cpus = 1            # CPU 1コア
  end
end
