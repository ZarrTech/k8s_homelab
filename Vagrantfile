Vagrant.configure("2") do |config|
  # General Vagrant configuration
  config.vm.box = "ubuntu/jammy64"
  config.vm.boot_timeout = 600
  config.vm.provision "shell", inline: <<-SHELL
  mkdir -p /home/vagrant/.ssh
  echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDhtMLutBgcpVciEK6JkALsXweLI6FfFcZAt5CvuK/KeW8QHG1jC2qKnN3flL8T/J2BN6Xj1NJJbKU/b6TGaj4E4+zq+ZVUsocc8MLkX/yW3MYmQeQEQTxl+E3cIuF+WTwfkCLt4AC3zQHXBeLuIkF07E3G2SgE1JMVVhJdh9wvWgHVkWlt1dC35l3Guoy85+G2pVATjHWDFSe8cv11pnuwZ5FKlTViJmVT2oAMJYv4Or8tRON7YFtUX/9GhPtt2Jw3BkI/yrdQFlpDbcMv+biUnTGIOtwK9b24yNpBFtc3HbUED3j/YUFrB8MweOOaKn1kcpZtwAEvKpmm6GHvgRje2Wav0OxoEJZBHqKqCQmB8NzCkeZImx108nHu848pSdxOvjmXy+QRjuUvVPMKHpCMKXbCIgdsSc9PILt3eec+AS4nvapOR13u3D8N2qCFl/9Zu+T8WycoWtr8ZrhIZEKGypU1XJY+uQhzXlN5HaZEkwgxCC3nCwGnopHgOAIkhlk= miel@2elve" >> /home/vagrant/.ssh/authorized_keys
  chmod 700 /home/vagrant/.ssh
  chmod 600 /home/vagrant/.ssh/authorized_keys
  chown -R vagrant:vagrant /home/vagrant/.ssh
SHELL


  # bastion server
  config.vm.define "bastion" do |bastion|
    bastion.vm.hostname = "bastion"
    bastion.vm.network "private_network", ip: "192.168.56.10"
    bastion.vm.network "public_network", bridge: "en0: Wi-Fi (Wireless)", auto_config: true
    bastion.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end

    bastion.vm.provision "shell", inline: <<-SHELL
      echo "192.168.56.9 remote" | sudo tee -a /etc/hosts
      echo "192.168.56.10 bastion" | sudo tee -a /etc/hosts
      echo "192.168.56.11 control" | sudo tee -a /etc/hosts
      echo "192.168.56.12 worker1" | sudo tee -a /etc/hosts
      echo "192.168.56.13 worker2" | sudo tee -a /etc/hosts
    SHELL
  end


 #remote server
  config.vm.define "remote" do |remote|
    remote.vm.hostname = "remote"
    remote.vm.network "private_network", ip: "192.168.56.9"
    remote.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end

  remote.vm.provision "shell", inline: <<-SHELL
    echo "192.168.56.9 remote" | sudo tee -a /etc/hosts
    echo "192.168.56.10 bastion" | sudo tee -a /etc/hosts
    echo "192.168.56.11 control" | sudo tee -a /etc/hosts
    echo "192.168.56.12 worker1" | sudo tee -a /etc/hosts
    echo "192.168.56.13 worker2" | sudo tee -a /etc/hosts
  SHELL
  end

  # Control plane node
  config.vm.define "control" do |control|
    control.vm.hostname = "control"
    control.vm.network "private_network", ip: "192.168.56.11"
    control.vm.provider "virtualbox" do |vb|
      vb.memory = 6096
      vb.cpus = 2
    end

    control.vm.provision "shell", inline: <<-SHELL
      echo "192.168.56.9 remote" | sudo tee -a /etc/hosts
      echo "192.168.56.10 bastion" | sudo tee -a /etc/hosts
      echo "192.168.56.11 control" | sudo tee -a /etc/hosts
      echo "192.168.56.12 worker1" | sudo tee -a /etc/hosts
      echo "192.168.56.13 worker2" | sudo tee -a /etc/hosts
    SHELL

    # Remove default NAT by overriding the first adapter
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--nic1", "none"]
    end
  end

  # Worker node 1
  config.vm.define "worker1" do |worker|
    worker.vm.hostname = "worker1"
    worker.vm.network "private_network", ip: "192.168.56.12"
    worker.vm.provider "virtualbox" do |vb|
      vb.memory = 8192
      vb.cpus = 2
    end

    worker.vm.provision "shell", inline: <<-SHELL
      echo "192.168.56.9 remote" | sudo tee -a /etc/hosts
      echo "192.168.56.10 bastion" | sudo tee -a /etc/hosts
      echo "192.168.56.11 control" | sudo tee -a /etc/hosts
      echo "192.168.56.12 worker1" | sudo tee -a /etc/hosts
      echo "192.168.56.13 worker2" | sudo tee -a /etc/hosts
    SHELL

    # Remove default NAT by overriding the first adapter
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--nic1", "none"]
    end
  end

  # Worker node 2
  config.vm.define "worker2" do |worker|
    worker.vm.hostname = "worker2"
    worker.vm.network "private_network", ip: "192.168.56.13"
    worker.vm.provider "virtualbox" do |vb|
      vb.memory = 8192
      vb.cpus = 2
    end

    worker.vm.provision "shell", inline: <<-SHELL
      echo "192.168.56.9 remote" | sudo tee -a /etc/hosts
      echo "192.168.56.10 bastion" | sudo tee -a /etc/hosts
      echo "192.168.56.11 control" | sudo tee -a /etc/hosts
      echo "192.168.56.12 worker1" | sudo tee -a /etc/hosts
      echo "192.168.56.13 worker2" | sudo tee -a /etc/hosts
    SHELL

    # Remove default NAT by overriding the first adapter
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--nic1", "none"]
    end
  end
end

