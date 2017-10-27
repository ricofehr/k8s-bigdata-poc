# k8s master
masters = {
    "master1" => "192.168.77.10"
}

# k8s nodes
nodes = {
    "node1" => "192.168.77.20",
    "node2" => "192.168.77.21"
}

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.ssh.forward_agent = true

    check_guest_additions = false
    functional_vboxsf = false

    config.vm.box = "bento/ubuntu-16.04"

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook.yml"
      ansible.inventory_path = "ansible/inventory"
    end

    masters.each do |name, ip|
      config.vm.define name do |machine|
        machine.vm.hostname = name
        machine.vm.network :private_network, ip: ip
        machine.vm.provider "virtualbox" do |v|
          v.name = name
          v.memory = 2048
          v.cpus = 1
        end
      end
    end

    nodes.each do |name, ip|
      config.vm.define name do |machine|
        machine.vm.hostname = name
        machine.vm.network :private_network, ip: ip
        machine.vm.provider "virtualbox" do |v|
          v.name = name
          v.memory = 2048
          v.cpus = 2
        end
      end
    end
end
