# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_version = "20150430.0.0"

  # Keep insecure keypair
  config.ssh.insert_key = false

  config.vm.provision "shell", privileged: true, inline: <<-SHELL

    # Install docker-compose
    curl -L https://github.com/docker/compose/releases/download/1.2.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose

    # Install go
    curl -O https://storage.googleapis.com/golang/go1.4.2.linux-amd64.tar.gz
    tar -C /usr/local -xzf go1.4.2.linux-amd64.tar.gz
    echo export PATH=\\\$PATH:/usr/local/go/bin >> /etc/profile

    # Install mongodb
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
    echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.0.list
    apt-get update
    apt-get install -y mongodb-org

    # Install node.js
    apt-get install -y build-essential
    curl -sL https://github.com/taaem/nodejs-linux-installer/releases/download/0.2/node-install.sh | bash

    # Install git
    apt-get install -y git
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL

    # Setup go workspace, see https://golang.org/doc/code.html
    grep '^export GOPATH' ~/.bashrc || echo export GOPATH=~/go >> ~/.bashrc
    grep '^export PATH' ~/.bashrc || echo export PATH=\\\$PATH:~/go/bin >> ~/.bashrc

    # Install godep, see https://github.com/tools/godep
    GOPATH=~/go /usr/local/go/bin/go get github.com/tools/godep
  SHELL

  # Install latest docker
  config.vm.provision :docker do |d|
  end
end
