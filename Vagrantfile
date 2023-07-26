# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Box and box version settings
  config.vm.box = "ubuntu/focal64"
  config.vm.provider "virtualbox"

  # Disable generating a new SSH key for each VM
  config.ssh.insert_key = false
  
  # Default settings for the VirtualBox provider
  config.vm.provider :virtualbox do |vb|
    vb.memory = 1024
    vb.cpus = 1
    vb.linked_clone = true
  end
  
  # Define the VMs with static private IP addresses.
  vms = [
    { name: "master", ip: "192.168.33.31" },
    { name: "node1", ip: "192.168.33.32" },
    { name: "node2", ip: "192.168.33.33" }
  ]

  # Install Ansible & dependencies on the primary node (master).
  ansible_script = <<-'SCRIPT'
    sudo apt-get update
    sudo apt-get -y install ansible
  SCRIPT

  vms.each do |vm|
    config.vm.define vm[:name] do |node|
      node.vm.hostname = vm[:name]
      node.vm.network :private_network, ip: vm[:ip]

  
      # Provision the primary node (master) with Ansible and copy the hosts file.
      if vm[:name] == "master"
        node.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "~/.ssh/id_rsa"
        node.vm.provision :shell, inline: "chmod 600 ~/.ssh/id_rsa", privileged: false
        node.vm.provision :shell, inline: ansible_script, privileged: true

        node.vm.synced_folder "./", "/home/vagrant/playbook", mount_options: ["dmode=755", "fmode=644"], create: true

        # Generate entries for /etc/hosts dynamically based on the 'vms' array.
        hosts_entries = vms.map { |vm_entry| "#{vm_entry[:ip]} #{vm_entry[:name]}" }.join("\n")
        node.vm.provision :shell, inline: "echo '#{hosts_entries}' | sudo tee /etc/hosts"

      end
    end
  end
end
