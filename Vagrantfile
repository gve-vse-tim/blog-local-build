# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Which base image
  config.vm.box = "fedora/33-cloud-base"

  # Open ports to view web pages built within this box
  config.vm.network "forwarded_port", guest: 4000, host: 4000, host_ip: "127.0.0.1"

  # Sync the first path from the host into the second path in the VM
  config.vm.synced_folder "./gve-vse-tim.github.io", "/home/vagrant/site", owner: "vagrant", group: "vagrant"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Specify VM memory requirements (MB)
    vb.memory = "1024"

    # Specify VM vCPUs
    vb.cpus = 2

    # Some VMs are borked on "serial console" so hard code this
    vb.customize ["modifyvm", :id, "--uart1", "0x3f8", "4"]
    vb.customize ["modifyvm", :id, "--uartmode1", "tcpserver", "2023"]
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
  
  end

  # Provision the VM for a turnkey GitHub site development environment
  config.vm.provision "shell", inline: <<-SHELL
    dnf update -y
    dnf install -y @development-tools ruby-devel redhat-rpm-config zlib-devel
    bundle install --gemfile=/home/vagrant/site/Gemfile
    # su - vagrant -c "cd /home/vagrant; \
    # "
    echo "export MYHOST=\$(ip address show dev eth0 | awk '/global/ { print \$2; }' | cut -d/ -f1)\" >> /home/vagrant/.bashrc
  SHELL
end

### Links to related docs on building this Vagrantfile

## Requirements
# https://jekyllrb.com/docs/installation/

## Pre-requisites for Fedora
# https://jekyllrb.com/docs/installation/other-linux/

## GitHub Pages (build locally)
# https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll
