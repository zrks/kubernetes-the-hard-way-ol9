# -*- mode: ruby -*-
# vi: set ft=ruby :

# Configuration Variables
VM_CONFIG = {
  "bandmaster"      => { ip: "10", memory: 2048, cpus: 2 },
  "gitlab"          => { ip: "20", memory: 4096, cpus: 4 },
  "bind9"           => { ip: "30", memory: 1536, cpus: 1 },
  "loadbalancer"    => { ip: "40", memory: 1536, cpus: 1 },
  "controlplane1"   => { ip: "41", memory: 1536, cpus: 1 },
  "controlplane2"   => { ip: "42", memory: 1536, cpus: 1 },
  "worker1"         => { ip: "43", memory: 1536, cpus: 1 },
  "worker2"         => { ip: "44", memory: 1536, cpus: 1 }
}

IP_NW = "10.0.0."

# Common script to update /etc/hosts
$commonscript = <<-SCRIPT
entries=("10.0.0.10   bandmaster"
         "10.0.0.20   gitlab"
         "10.0.0.30   bind9"
         "10.0.0.40   loadbalancer"
         "10.0.0.41   controlplane1"
         "10.0.0.42   controlplane2"
         "10.0.0.43   worker1"
         "10.0.0.44   worker2")

for entry in "${entries[@]}"; do
  if ! grep -qF "$entry" /etc/hosts; then
    echo "$entry" | sudo tee -a /etc/hosts > /dev/null
  fi
done
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "generic/oracle9"

  # Reusable VM definition block
  VM_CONFIG.each do |name, cfg|
    config.vm.define name do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.name = name
        node.vm.hostname = name
        vb.memory = cfg[:memory]
        vb.cpus = cfg[:cpus]
      end
      node.vm.network :private_network, ip: IP_NW + cfg[:ip]
      node.vm.provision "shell", inline: $commonscript
    end
  end

  # Ansible Provisioning Block
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible_provisioning/playbook.yml"
    ansible.extra_vars = {
      target: VM_CONFIG.keys.join(',')
    }
    ansible.verbose = "v"
  end

  # Ansible for bandmaster
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible_provisioning/bandmaster_playbook.yml"
    ansible.extra_vars = {
      target: "bandmaster"
    }
    ansible.verbose = "v"
  end

end
