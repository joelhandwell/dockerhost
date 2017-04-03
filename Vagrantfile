Vagrant.configure("2") do |config|
  config.vm.box = "AlbanMontaigu/boot2docker"
  config.vm.provider "virtualbox" do |v|
    v.memory = 16384
    v.cpus = 16
  end
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "C:/Users", "/c/Users"
  config.vm.network "private_network", ip: "192.168.2.193"

  config.vm.provision "shell", inline: "sudo wget https://github.com/bcicen/ctop/releases/download/v0.5/ctop-0.5-linux-amd64 -O /usr/local/bin/ctop"
  config.vm.provision "shell", inline: "sudo chmod +x /usr/local/bin/ctop"
end
