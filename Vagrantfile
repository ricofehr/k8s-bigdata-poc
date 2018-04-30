# k8s master
masters = {
    "k8s-master1" => "192.168.87.10"
}

# k8s nodes
nodes = {
    "k8s-node1" => "192.168.87.20",
    "k8s-node2" => "192.168.87.21",
    "k8s-node3" => "192.168.87.22",
    "k8s-node4" => "192.168.87.23"
}

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.ssh.forward_agent = true

    check_guest_additions = false
    functional_vboxsf = false

    config.vm.box = "bento/ubuntu-16.04"

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
          v.memory = 12288
          v.cpus = 6

          unless File.exist?("/datas/k8s/#{name}/disk_osd-#{name}.vdi")
            v.customize [ "createhd", "--filename", "/datas/k8s/#{name}/disk_osd-#{name}", "--size", "40960" ]
          end
          v.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 3, "--device", 0, "--type", "hdd", "--medium", "/datas/k8s/#{name}/disk_osd-#{name}.vdi" ]
        end

        if name == "k8s-node4"
          machine.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/playbook.yml"
            ansible.inventory_path = "ansible/inventory"
            ansible.verbose = false
            ansible.limit = "all"
          end
        end
      end
    end
end
