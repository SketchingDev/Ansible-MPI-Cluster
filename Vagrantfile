# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
TOTAL_SLAVES = 1

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "parallels/ubuntu-14.04"
  slave_machine_ids = Array.new

  config.vm.define :master, primary: true do |master|
    master.vm.network :private_network, ip: "192.168.59.11"
    master.vm.hostname = "master"
  end

  (1..TOTAL_SLAVES).each do |machine_id|
    config.vm.define "slave#{machine_id}" do |slave|
      slave.vm.network :private_network, ip: "192.168.59.#{20+machine_id}"
      slave.vm.hostname = "slave#{machine_id}"

      # Collect slave hostnames for ansible group below
      slave_machine_ids.push slave.vm.hostname

      # Only execute once the Ansible provisioner, when all the machines are up and ready.
      if machine_id == TOTAL_SLAVES
        slave.vm.provision :ansible do |ansible|
          ansible.playbook = "provisioning/site.yml"
          ansible.limit = "all"
          ansible.groups = {
             "slaves" => slave_machine_ids
          }
        end
      end
    end
  end
end
