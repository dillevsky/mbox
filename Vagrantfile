Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.name = "Management Box"
    v.memory = 1024
    v.cpus = 2
    v.customize ["modifyvm", :id, "--audio", "none"]
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end
  config.vm.box = "centos/7"
  config.vm.hostname = "management-box"
  config.vm.network "private_network", type: "dhcp"
  config.vm.provision "file",
    source: "~/.ssh/id_rsa",
    destination: "/home/vagrant/.ssh/id_rsa"
  config.vm.provision "file",
    source: "~/.ssh/id_rsa.pub",
    destination: "/home/vagrant/.ssh/id_rsa.pub"
  config.vm.provision "file",
    source: "~/.aws",
    destination: "/home/vagrant/"
  config.vm.provision "shell",
    inline: '
      yum install -y epel-release;
      yum -y update;
      yum -y install python3 python3-setuptools python3-devel python3-pip;
      python3 -m pip install ansible;
      chmod 600 /home/vagrant/.ssh/id_rsa;
      cat /home/vagrant/.ssh/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys;
      chmod 600 /home/vagrant/.aws/*
    '
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "management-box.yml"
  end
end
