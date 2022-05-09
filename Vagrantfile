# coding: utf-8
# -*- mode: ruby -*-
Vagrant.configure(2) do |config|
  # プロキシ設定
  #if Vagrant.has_plugin?("vagrant-proxyconf")
  # config.proxy.enabled  = true  # => true; all applications enabled, false; all applications disabled
  # config.proxy.http     = ""
  # config.proxy.https    = ""
  #  config.proxy.no_proxy = "localhost,127.0.0.1,172.16.20.11,172.16.20.12,172.16.20.13,10.96.0.0/12,10.244.0.0/16,10.32.0.10" 
  #end

  #if Vagrant.has_plugin?("vagrant-vbguest")
  #  config.vbguest.auto_update = false
  #end

  # マスタ 仮想マシンの起動
  config.vm.define 'master' do |machine|
    machine.vm.box = "centos/7"
    machine.vm.hostname = 'master'
    machine.vm.network :private_network,ip: "192.168.56.11"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 4
      vbox.memory = 8192
      vbox.customize [
        "modifyvm", :id,
        "--nested-hw-virt", "on",
        "--paravirtprovider", "kvm",
      ]
    end
    machine.vm.synced_folder "/mnt/c/tmp/sync", "/home/vagrant/sync", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=777", "fmode=777"]

    # マスタ docker & k8sのインストール
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/kubernetes.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.limit = "master"
      ansible.inventory_path = "ansible-playbook/hosts"
    end

    # マスタのセットアップ
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/k8s_master.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.install = false
      ansible.limit = "master"
      ansible.inventory_path = "ansible-playbook/hosts"
      ansible.start_at_task = ENV['restart containerd']
    end
  end

  # Worker Node１ 仮想マシンの起動
  config.vm.define 'worker1' do |machine|
    machine.vm.box = "centos/7"
    machine.vm.hostname = 'worker1'
    machine.vm.network :private_network,ip: "192.168.56.12"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 4
      vbox.memory = 8192
    end
    machine.vm.synced_folder "/mnt/c/tmp/sync", "/home/vagrant/sync", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=777", "fmode=777"]

    # Worker Node１ docker & k8sのインストール
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/kubernetes.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.limit = "worker1"
      ansible.inventory_path = "ansible-playbook/hosts"
    end

    # ノードのセットアップ
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/k8s_workers.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.install = false
      ansible.limit = "worker1"
      ansible.inventory_path = "ansible-playbook/hosts"
    end
  end

  # Worker Node２ 仮想マシンの起動
  config.vm.define 'worker2' do |machine|
    machine.vm.box = "centos/7"
    machine.vm.hostname = 'worker2'
    machine.vm.network :private_network,ip: "192.168.56.13"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 4
      vbox.memory = 8192
    end
    machine.vm.synced_folder "/mnt/c/tmp/sync", "/home/vagrant/sync", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=777", "fmode=777"]

    # Worker Node２ docker & k8sのインストール
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/kubernetes.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.limit = "worker2"
      ansible.inventory_path = "ansible-playbook/hosts"
    end

    # ノードのセットアップ
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/k8s_workers.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.install = false
      ansible.limit = "worker2"
      ansible.inventory_path = "ansible-playbook/hosts"
    end
  end

  # Worker Node３ 仮想マシンの起動
  config.vm.define 'worker3' do |machine|
    machine.vm.box = "centos/7"
    machine.vm.hostname = 'worker3'
    machine.vm.network :private_network,ip: "192.168.56.14"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.cpus = 4
      vbox.memory = 8192
    end
    machine.vm.synced_folder "/mnt/c/tmp/sync", "/home/vagrant/sync", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=777", "fmode=777"]

    # Worker Node３ docker & k8sのインストール
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/kubernetes.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.limit = "worker3"
      ansible.inventory_path = "ansible-playbook/hosts"
    end

    # ノードのセットアップ
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible-playbook/k8s_workers.yml"
      ansible.version = "latest"
      ansible.verbose = false
      ansible.install = false
      ansible.limit = "worker3"
      ansible.inventory_path = "ansible-playbook/hosts"
    end
  end
end
