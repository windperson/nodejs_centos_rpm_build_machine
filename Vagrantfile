# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  def createBuildMachine(config, osver, memory_size, cpu_cores)
      config.vm.box = "CentOS#{osver}"
      config.ssh.insert_key = false
      @nodeName = "nodejs-rpm-build-centos#{osver}"
      config.vm.hostname = @nodeName
      config.vm.define @nodeName
      config.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        vb.gui = false
        vb.customize ["modifyvm", :id, "--memory", "#{memory_size}"]
        vb.customize ["modifyvm", :id, "--cpus", "#{cpu_cores}"]
      end
      # install necessary packages for build node.js rpm.
      if osver >= 7
        config.vm.provision "shell", inline: <<-SHELL
          sudo yum install -y gcc-c++ git make openssl-devel yum-utils rpmdevtools
        SHELL
      else
        config.vm.provision "shell", inline: <<-SHELL
          sudo yum install -y scl-utils &&
          sudo yum install -y /vagrant/rhscl-python27-epel-6-x86_64-1-2.noarch.rpm \
          /vagrant/rhscl-devtoolset-3-epel-6-x86_64-1-2.noarch.rpm &&
          sudo yum install -y git openssl-devel yum-utils rpmdevtools python27
        SHELL
      end

  end

  config.vm.define "CentOS7", primary: true do |bm|
    createBuildMachine(bm,7,4096,2)
  end

  config.vm.define "CentOS6.5", autostart: false do |bm|
    createBuildMachine(bm,6.5,4096,2)
  end

end
