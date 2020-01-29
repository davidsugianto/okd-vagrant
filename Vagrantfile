# -*- mode: ruby -*-
# vi: set ft=ruby :

# NETWORK_BASE = "172.28.128"
# INTEGRATION_START_SEGMENT = 11
LB_MASTER_IP = "172.28.128.10"
LB_INFRA_IP = "172.28.128.11"
MASTER_ONE_IP = "172.28.128.12"
MASTER_TWO_IP = "172.28.128.13"
INFRA_ONE_IP = "172.28.128.14"
INFRA_TWO_IP = "172.28.128.15"
APP_ONE_IP = "172.28.128.16"
APP_TWO_IP = "172.28.128.17"
GLUSTER_IP = "172.28.128.18"
# NODE_COUNT = 2

VAGRANT_BOX = 'centos/7'

Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false

  config.vm.provision "file", source: "./okd-ssh/authorized_keys", destination: "/home/vagrant/.ssh/authorized_keys"
  config.vm.provision "file", source: "./okd-ssh/config", destination: "/home/vagrant/.ssh/config"
  config.vm.provision "shell", inline: <<-SHELL
    sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
    sudo systemctl restart sshd.service
    echo "finished ssh restarted"
  SHELL

  # Define loadbalancer master node
  config.vm.define "lb-master-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    config.disksize.size = '15GB'
    app.vm.hostname = "lb-master.openshift.local"
    app.vm.network "private_network", ip: "#{LB_MASTER_IP}"
    app.vm.provider "virtualbox" do |v|
      v.name = "lb-master-openshift"
      v.memory = 512
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/lb-master.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/lb-master", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end

  # Define loadbalancer infra node
  config.vm.define "lb-infra-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    config.disksize.size = '15GB'
    app.vm.hostname = "lb-infra.openshift.local"
    app.vm.network "private_network" ip: "#{LB_INFRA_IP}"
    app.vm.provider "virtualbox" do |v|
      v.name = "lb-infra-openshift"
      v.memory = 512
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/lb-infra.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/lb-infra", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end

  # Define master node 1
  config.vm.define "master-one-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    app.vm.hostname = "master-one.openshift.local"
    app.vm.network "private_network" ip: "#{MASTER_ONE_IP}"
    app.vm.provision "file", source: "./okd-setup/ansible.cfg", destination: "/opt/ansible.cfg"
    app.vm.provider "virtualbox" do |v|
      v.name = "master-one-openshift"
      v.memory = 2048
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/master-one.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/master-one", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end
  
  # Define master node 2
  config.vm.define "master-two-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    app.vm.hostname = "master-two.openshift.local"
    app.vm.network "private_network" ip: "#{MASTER_TWO_IP}"
    app.vm.provision "file", source: "./okd-setup/ansible.cfg", destination: "/opt/ansible.cfg"
    app.vm.provider "virtualbox" do |v|
      v.name = "master-two-openshift"
      v.memory = 2048
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/master-two.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/master-two", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end

  # Define infra node 1
  config.vm.provision "infra-one-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    config.disksize.size = '20GB'
    app.vm.hostname = "infra-one.openshift.local"
    app.vm.network "provate_network" ip: "#{INFRA_ONE_IP}"
    app.vm.provision "file", source: "./okd-setup/ansible.cfg", destination: "/opt/ansible.cfg"
    app.vm.provider "virtualbox" do |v|
      v.name = "infra-one-openshift"
      v.memory = 1024
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/infra-one.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/infra-one", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end

  # Define infra node 2
  config.vm.provision "infra-two-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    config.disksize.size = '20GB'
    app.vm.hostname = "infra-two.openshift.local"
    app.vm.network "provate_network" ip: "#{INFRA_TWO_IP}"
    app.vm.provision "file", source: "./okd-setup/ansible.cfg", destination: "/opt/ansible.cfg"
    app.vm.provider "virtualbox" do |v|
      v.name = "infra-two-openshift"
      v.memory = 1024
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/infra-two.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/infra-two", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end

  # Define app node 1
  config.vm.provision "app-one-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    config.disksize.size = '20GB'
    app.vm.hostname = "app-one.openshift.local"
    app.vm.network "provate_network" ip: "#{APP_ONE_IP}"
    app.vm.provision "file", source: "./okd-setup/ansible.cfg", destination: "/opt/ansible.cfg"
    app.vm.provider "virtualbox" do |v|
      v.name = "app-one-openshift"
      v.memory = 1024
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/app-one.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/app-one", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end

  # Define app node 2
  config.vm.provision "app-two-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    config.disksize.size = '20GB'
    app.vm.hostname = "app-two.openshift.local"
    app.vm.network "provate_network" ip: "#{APP_TWO_IP}"
    app.vm.provision "file", source: "./okd-setup/ansible.cfg", destination: "/opt/ansible.cfg"
    app.vm.provider "virtualbox" do |v|
      v.name = "app-two-openshift"
      v.memory = 1024
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/app-two.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/app-two", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end

  # Define gluster node
  config.vm.provision "gluster-one-openshift" do |app|
    app.vm.box = VAGRANT_BOX
    config.disksize.size = '20GB'
    app.vm.hostname = "gluster-one.openshift.local"
    app.vm.network "provate_network" ip: "#{GLUSTER_IP}"
    app.vm.provision "file", source: "./okd-setup/ansible.cfg", destination: "/opt/ansible.cfg"
    app.vm.provider "virtualbox" do |v|
      v.name = "gluster-one-openshift"
      v.memory = 1024
      v.cpus = 1
    end
    app.vm.provision "file", source: "./okd-ssh/gluster-one.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
    app.vm.provision "file", source: "./okd-ssh/gluster-one", destination: "/home/vagrant/.ssh/id_rsa"
    app.vm.provision "shell", inline: <<-SHELL
      chmod 400 /home/vagrant/.ssh/id_rsa
      chmod 400 /home/vagrant/.ssh/id_rsa.pub
      sudo systemctl restart sshd.service
      echo "finished ssh key permission"
    SHELL
  end
end
