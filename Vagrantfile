NODE_NAME = "k3s-master"
IMAGE_NAME = "dev-env" 

Vagrant.configure("2") do |config|
  config.vm.provider "vmware_desktop" do |v|
    v.gui = true
    v.vmx["memsize"] = "8192"  
    v.vmx["numvcpus"] = "4"
  end

  config.ssh.insert_key = true
  config.ssh.forward_agent = true

  config.vm.network "forwarded_port",
  guest: 8001,
  host:  8001,
  auto_correct: true

  config.vm.network "forwarded_port",
  guest: 30000,
  host:  30000,
  auto_correct: true

  config.vm.network "forwarded_port",
  guest: 8443,
  host:  8443,
  auto_correct: true

  config.vm.network "forwarded_port",
  guest: 8080,
  host:  8080,
  auto_correct: true


  config.vm.define NODE_NAME do |master|     

    master.vm.box = IMAGE_NAME
    master.vm.network "private_network", ip: "192.168.50.10"
    master.vm.hostname = NODE_NAME
    master.vm.provision "ansible_local" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "install-minikube.yaml"
        ansible.extra_vars = {
            node_ip: "192.168.50.10",
        }
    end
  end

  config.vm.synced_folder "./server-data", "/home/vagrant/server-data"
end

