# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  boxes = [
        { :name => "k8master", :ip => "192.168.50.230" },
        { :name => "k8node1", :ip => "192.168.50.231" },
        { :name => "k8node2", :ip => "192.168.50.232" }
    ]
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  boxes.each do |opts|
    config.vm.define opts[:name] do |node|
      node.vm.box = "ubuntu/focal64"
      node.vm.hostname = opts[:name]
      node.vm.network :public_network, bridge: "wlp6s0", ip: opts[:ip]

      node.vm.provider "virtualbox" do |vb3|
        #   Display the VirtualBox GUI when booting the machine
        #vb3.gui = true
        #vb3.name = "k8node2"
        vb3.cpus = 2
        #   Customize the amount of memory on the VM:
        vb3.memory = "4000"
        #vb.customize ["modifyvm", :id, "--memory", 2048]
        #vb.customize ["modifyvm", :id, "--cpus", 2]
      end
    if opts[:name] == "k8node2"
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.extra_vars = {
          K8S_MASTER_NODE_IP: "192.168.50.230",
          K8S_NODE1_IP: "192.168.50.231",
          K8S_NODE2_IP: "192.168.50.232",
          "K8S_API_SECURE_PORT" => 6443}
        ansible.limit = "all"
        ansible.groups = {
                "k8snodes" => ["k8node1", "k8node2"],
                "k8s" => ["k8master", "k8node1", "k8node2"],
                "k8smaster" => ["k8master"],
                "k8snodes:vars" => {"K8S_MASTER_NODE_IP=" => "192.168.50.230",
                                  "K8S_API_SECURE_PORT" => 6443}
                                  }
                                  
        end
      end
    end
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
   #config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #vb.name = "k8s-master"
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
   #end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
