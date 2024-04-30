# -*- mode: ruby -*-
# vi: set ft=ruby :

BANDMASTER_VM_NAME = "bandmaster"
GIT_VM_NAME = "gitlab"
DNS_VM_NAME = "bind9"

LOADBALANCER_NAME = "loadbalancer"

CONTROLPLANE_1_NAME = "controlplane1"
CONTROLPLANE_2_NAME = "controlplane2"

WORKER_1_NAME = "worker1"
WORKER_2_NAME = "worker2"

# N.B.
# https://www.virtualbox.org/manual/ch06.html#network_hostonly
IP_NW = "10.0.0."
BANDMASTER_HOST = "10"
GIT_HOST = "20"
DNS_HOST = "30"
LOADBALANCER_HOST = "40"
CONTROLPLANE_1_HOST = "41"
CONTROLPLANE_2_HOST = "42"
WORKER_1_HOST = "43"
WORKER_2_HOST = "44"

$commonscript = <<-SCRIPT
sudo echo "10.0.0.10   bandmaster" >> /etc/hosts
sudo echo "10.0.0.20   gitlab" >> /etc/hosts
sudo echo "10.0.0.30   bind9" >> /etc/hosts
sudo echo "10.0.0.40   loadbalancer" >> /etc/hosts
sudo echo "10.0.0.41   controlplane1" >> /etc/hosts
sudo echo "10.0.0.42   controlplane2" >> /etc/hosts
sudo echo "10.0.0.43   worker1" >> /etc/hosts
SCRIPT


# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.

  # System requirements for Oracle Linux 9:
  # https://docs.oracle.com/en/operating-systems/oracle-linux/9/install/install-PreparingToInstall.html#prep-install
  config.vm.box = "generic/oracle9"

  config.vm.define BANDMASTER_VM_NAME do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = BANDMASTER_VM_NAME
      node.vm.hostname = BANDMASTER_VM_NAME
      vb.memory = 2048
      vb.cpus = 2
    end
    node.vm.network :private_network, ip: IP_NW + BANDMASTER_HOST
    node.vm.provision "shell", inline: $commonscript
    # TODO: Generate ssh key and distribute it to all other machines
  end

  config.vm.define GIT_VM_NAME do |node|
    # TODO:
    # https://gitlab.com/rluna-gitlab/gitlab-ce/-/blob/master/doc/install/requirements.md
    node.vm.provider "virtualbox" do |vb|
      vb.name = GIT_VM_NAME
      node.vm.hostname = GIT_VM_NAME
      vb.memory = 4096
      vb.cpus = 4
    end
    node.vm.network :private_network, ip: IP_NW + GIT_HOST
    node.vm.provision "shell", inline: $commonscript
  end

  config.vm.define DNS_VM_NAME do |node|
    # TODO:
    # https://docs.oracle.com/en/operating-systems/oracle-linux/8/network/network-ConfiguringtheNameService.html#ol-namesvc-resource-records
    node.vm.provider "virtualbox" do |vb|
      vb.name = DNS_VM_NAME
      node.vm.hostname = DNS_VM_NAME
      vb.memory = 1536
      vb.cpus = 1
    end
    node.vm.network :private_network, ip: IP_NW + DNS_HOST
    node.vm.provision "shell", inline: $commonscript
  end

  config.vm.define LOADBALANCER_NAME do |node|
    # TODO:
    # https://docs.oracle.com/en/operating-systems/oracle-linux/8/network/network-ConfiguringtheNameService.html#ol-namesvc-resource-records
    node.vm.provider "virtualbox" do |vb|
      vb.name = LOADBALANCER_NAME
      node.vm.hostname = LOADBALANCER_NAME
      vb.memory = 1536
      vb.cpus = 1
    end
    node.vm.network :private_network, ip: IP_NW + LOADBALANCER_HOST
    node.vm.provision "shell", inline: $commonscript
  end

  config.vm.define CONTROLPLANE_1_NAME do |node|
    # TODO:
    # https://docs.oracle.com/en/operating-systems/oracle-linux/8/network/network-ConfiguringtheNameService.html#ol-namesvc-resource-records
    node.vm.provider "virtualbox" do |vb|
      vb.name = CONTROLPLANE_1_NAME
      node.vm.hostname = CONTROLPLANE_1_NAME
      vb.memory = 1536
      vb.cpus = 1
    end
    node.vm.network :private_network, ip: IP_NW + CONTROLPLANE_1_HOST
    node.vm.provision "shell", inline: $commonscript
  end

  config.vm.define CONTROLPLANE_2_NAME do |node|
    # TODO:
    # https://docs.oracle.com/en/operating-systems/oracle-linux/8/network/network-ConfiguringtheNameService.html#ol-namesvc-resource-records
    node.vm.provider "virtualbox" do |vb|
      vb.name = CONTROLPLANE_2_NAME
      node.vm.hostname = CONTROLPLANE_2_NAME
      vb.memory = 1536
      vb.cpus = 1
    end
    node.vm.network :private_network, ip: IP_NW + CONTROLPLANE_2_HOST
    node.vm.provision "shell", inline: $commonscript
  end

  config.vm.define WORKER_1_NAME do |node|
    # TODO:
    # https://docs.oracle.com/en/operating-systems/oracle-linux/8/network/network-ConfiguringtheNameService.html#ol-namesvc-resource-records
    node.vm.provider "virtualbox" do |vb|
      vb.name = WORKER_1_NAME
      node.vm.hostname = WORKER_1_NAME
      vb.memory = 1536
      vb.cpus = 1
    end
    node.vm.network :private_network, ip: IP_NW + WORKER_1_HOST
    node.vm.provision "shell", inline: $commonscript
  end

  config.vm.define WORKER_2_NAME do |node|
    # TODO:
    # https://docs.oracle.com/en/operating-systems/oracle-linux/8/network/network-ConfiguringtheNameService.html#ol-namesvc-resource-records
    node.vm.provider "virtualbox" do |vb|
      vb.name = WORKER_2_NAME
      node.vm.hostname = WORKER_2_NAME
      vb.memory = 1536
      vb.cpus = 1
    end
    node.vm.network :private_network, ip: IP_NW + WORKER_2_HOST
    node.vm.provision "shell", inline: $commonscript
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible_provisioning/playbook.yml"
    ansible.extra_vars = {
      #role: "kernel", #"hello_server", #"provision",
      target: BANDMASTER_VM_NAME+","+GIT_VM_NAME+","+DNS_VM_NAME+","+LOADBALANCER_NAME+","+CONTROLPLANE_1_NAME+","+CONTROLPLANE_2_NAME+","+WORKER_1_NAME+","+WORKER_2_NAME,
    }
    ansible.verbose = "v"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible_provisioning/bandmaster_playbook.yml"
    ansible.extra_vars = {
      target: BANDMASTER_VM_NAME
    }
    ansible.verbose = "v"
  end


  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
