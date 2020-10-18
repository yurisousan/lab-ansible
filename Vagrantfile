# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: <<-SHELL
#GERAL
    echo "127.0.0.1 localhost
192.168.99.10 appdocker
192.168.99.11 webapp
192.168.99.12 dbapp
192.168.99.13 ansible" > /etc/hosts
    mkdir /root/.ssh/
    echo "-----BEGIN RSA PRIVATE KEY-----
            <PRIVATE_KEY_HERE>
          -----END RSA PRIVATE KEY-----" >> /root/.ssh/id_rsa
    echo "ssh-rsa <RSA_KEY>" >> /root/.ssh/authorized_keys
            ---END RSA PRIVATE KEY-----" >> /root/.ssh/id_rsa
    echo "ssh-rsa <RSA_KEY_HERE>" >> /root/.ssh/authorized_keys

    chmod 600 /root/.ssh/id_rsa
  SHELL

#APP
  config.vm.define "appdocker" do |appdocker|
    appdocker.vm.hostname = "app"
    appdocker.vm.box = "centos/7"
    appdocker.vm.box_check_update = false
    appdocker.vm.network "private_network", ip: "192.168.99.10", dns: "8.8.8.8"

  appdocker.vm.provision "shell", inline: <<-SHELL
    yum update -y
  SHELL

    config.vm.provider "virtualbox" do |appdocker|
      appdocker.memory = "1024"
    end
  end

#WEBAPP
  config.vm.define "webapp" do |webapp|
    webapp.vm.hostname = "webapp"
    webapp.vm.box = "centos/7"
    webapp.vm.box_check_update = false
    webapp.vm.network "private_network", ip: "192.168.99.11", dns: "8.8.8.8"

  webapp.vm.provision "shell", inline: <<-SHELL
    yum update -y
  SHELL

    config.vm.provider "virtualbox" do |webapp|
      webapp.memory = "1024"
    end
  end

#DBAPP
  config.vm.define "dbapp" do |dbapp|
    dbapp.vm.hostname = "dbapp"
    dbapp.vm.box = "centos/7"
    dbapp.vm.box_check_update = false
    dbapp.vm.network "private_network", ip: "192.168.99.12", dns: "8.8.8.8"

  dbapp.vm.provision "shell", inline: <<-SHELL
    yum update -y
  SHELL

    dbapp.vm.provider "virtualbox" do |dbapp|
      dbapp.memory = "1024"
    end
  end

#ANSIBLE
  config.vm.define "ansible" do |ansible|
    ansible.vm.hostname = "ansible"
    ansible.vm.box = "centos/7"
    ansible.vm.box_check_update = false
    ansible.vm.network "private_network", ip: "192.168.99.13", dns: "8.8.8.8"

  ansible.vm.provision "shell", inline: <<-SHELL
    yum update -y
  SHELL

    config.vm.provider "virtualbox" do |ansible|
      ansible.memory = "1024"
    end
  end

end