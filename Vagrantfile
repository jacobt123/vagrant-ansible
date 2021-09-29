Vagrant.configure('2') do |config|
    config.vm.box = "centos/7"
    config.vm.hostname = "ansible-controller"
    config.vm.define "ansible-controller"
    config.vm.network :private_network, ip: "10.0.0.250"
    config.vm.provider :virtualbox do |vb|
        vb.name = "ansible-controller"
    end
    config.vm.provision "shell", inline: <<-SHELL
      sudo yum -y install epel-release
      sudo yum -y install ansible
      echo "127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4">/etc/hosts
      echo "::1         localhost localhost.localdomain localhost6 localhost6.localdomain6">>/etc/hosts
      echo "10.0.0.250  ansible-controller">>/etc/hosts
      echo "10.0.0.251  target01">>/etc/hosts
      echo "10.0.0.252  target02">>/etc/hosts
    SHELL
    config.vm.provision "file", source: "id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
    public_key = File.read("id_rsa.pub")
    config.vm.provision :shell, :inline =>"
    echo 'Copying ansible-vm public SSH Keys to the VM'
    mkdir -p /home/vagrant/.ssh
    chmod 700 /home/vagrant/.ssh
    echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
    chmod -R 600 /home/vagrant/.ssh/authorized_keys
    echo 'Host 10.0.*.*' >> /home/vagrant/.ssh/config
    echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
    echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
    chmod -R 600 /home/vagrant/.ssh/config", privileged: false
end
