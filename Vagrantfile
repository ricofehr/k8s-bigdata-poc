# k8s master
masters = {
    "kubespark-master1" => "192.168.77.10"
}

# k8s nodes
nodes = {
    "kubespark-node1" => "192.168.77.20",
    "kubespark-node2" => "192.168.77.21",
    "kubespark-node3" => "192.168.77.22",
    "kubespark-node4" => "192.168.77.23"
}

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.ssh.forward_agent = true

    check_guest_additions = false
    functional_vboxsf = false

    config.vm.box = "bento/ubuntu-16.04"

    # config.vm.provision "ansible" do |ansible|
    #   ansible.playbook = "ansible/playbook.yml"
    #   ansible.inventory_path = "ansible/inventory"
    # end

    masters.each do |name, ip|
      config.vm.define name do |machine|
        machine.vm.hostname = name
        machine.vm.network :private_network, ip: ip
        machine.vm.provider :virtualbox do |v|
          v.name = name
          v.memory = 6144
          v.cpus = 4
        end
      end
    end

    nodes.each do |name, ip|
      config.vm.define name do |machine|
        machine.vm.hostname = name
        machine.vm.network :private_network, ip: ip
        machine.vm.provider :virtualbox do |v|
          v.name = name
          v.memory = 4096
          v.cpus = 4
        end

        if name == "kubespark-node4"
          machine.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/playbook.yml"
            ansible.inventory_path = "ansible/inventory"
            ansible.verbose = false
            ansible.limit = "all"
#            ansible.tags = "k8s"
          end
        end
      end
    end
end
