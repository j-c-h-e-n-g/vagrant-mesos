# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|

  config.vm.network "forwarded_port", guest: 5050, host: 5050
  config.vm.box = "ubuntu-trusty64"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
  config.vm.provision "shell", inline: <<-SHELL

    # Add the Mesosphere repository
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
    DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
    CODENAME=$(lsb_release -cs)

    echo "deb http://repos.mesosphere.io/${DISTRO} ${CODENAME} main" | \
      sudo tee /etc/apt/sources.list.d/mesosphere.list

    # Install Java 8 from Oracle's PPA
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update -y

    sudo DEBIAN_FRONTEND=noninteractive apt-get install -y  python-software-properties software-properties-common mesos marathon

    sudo echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
    sudo apt-get install -y oracle-java8-installer oracle-java8-set-default

    echo 1 | sudo tee /etc/zookeeper/conf/myid
    echo server.1=127.0.0.1:2888:3888 | sudo tee  /etc/zookeeper/conf/zoo.cfg
    echo zk://127.0.0.1:2181/mesos | sudo tee   /etc/mesos/zk
    echo 1 | sudo tee /etc/mesos-master/quorum
    echo manual | sudo tee /etc/init/mesos-slave.override

    sudo service zookeeper restart
    sudo service mesos-master restart
    sudo service mesos-slave stop

  SHELL



end
