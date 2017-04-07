Vagrant.configure("2") do |config|
  config.vm.box = "joelhandwell/ubuntu_xenial64_vbguest"
  config.vm.provider "virtualbox" do |v|
    v.memory = 16384
    v.cpus = 16
  end
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "C:/Users", "/c/Users"
  config.vm.network "private_network", ip: "192.168.2.193"

  # install docker
  config.vm.provision "shell", inline: "sudo apt-get install apt-transport-https ca-certificates curl software-properties-common"
  config.vm.provision "shell", inline: "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -"
  config.vm.provision "shell", inline: 'sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial edge"'
  config.vm.provision "shell", inline: "sudo apt-get update"
  config.vm.provision "shell", inline: "sudo apt-get install -y linux-image-extra-$(uname -r) linux-image-extra-virtual"
  config.vm.provision "shell", inline: "sudo apt-get install -y docker-ce"

  # user vagrant do not need sudo to run docker
  config.vm.provision "shell", inline: "sudo gpasswd -a vagrant docker"
  config.vm.provision "shell", inline: "sudo service docker restart"
  config.vm.provision "shell", inline: 'su - vagrant -c "docker ps"'

  # install ctop
  config.vm.provision "shell", inline: "sudo wget -q https://github.com/bcicen/ctop/releases/download/v0.5.1/ctop-0.5.1-linux-amd64 -O /usr/local/bin/ctop"
  config.vm.provision "shell", inline: "sudo chmod +x /usr/local/bin/ctop"
end
