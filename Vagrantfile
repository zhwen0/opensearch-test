Vagrant.configure("2") do |config|
  config.vm.box = "generic/rhel7"
  config.vm.box_check_update = false

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
  end
  
  (1..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.provider "virtualbox" do |v|
        v.name = "opensearch_node#{i}"
        v.memory = "2048"
      end
      
      node.vm.synced_folder ".", "/vagrant", type:"virtualbox"
      
      node.vm.network "private_network", ip: "192.168.100.#{i+10}"
      node.vm.hostname = "node#{i}"

      node.vm.provision "shell", inline: <<-SHELL
        sudo cp /vagrant/files/hosts /etc/hosts
        sudo rpm --import /vagrant/files/opensearch.pgp
        sudo yum -y install /vagrant/files/opensearch-2.1.0-linux-x64.rpm
      SHELL
    end
  end
  
  config.vm.define "kb" do |node|
    node.vm.provider "virtualbox" do |v|
      v.name = "kb"
      v.memory = "2048"
    end
    
    node.vm.synced_folder ".", "/vagrant", type:"virtualbox"
    
    node.vm.network "private_network", ip: "192.168.100.21"
    node.vm.hostname = "kb"

    node.vm.provision "shell", inline: <<-SHELL
      sudo cp /vagrant/files/hosts /etc/hosts
      sudo rpm --import /vagrant/files/opensearch.pgp
      sudo yum -y install /vagrant/files/opensearch-dashboards-2.1.0-linux-x64.rpm
    SHELL
  end
  
end
