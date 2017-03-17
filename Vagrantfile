Vagrant.configure("2") do |config|
  config.vm.box = "AlbanMontaigu/boot2docker"
  config.vm.provider "virtualbox" do |v|
    v.memory = 8192
    v.cpus = 8
  end
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "C:/Users", "/c/Users"
  config.vm.network "private_network", ip: "192.168.99.103"
end
