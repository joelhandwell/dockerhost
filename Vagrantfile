Vagrant.configure("2") do |config|
  config.vm.box = "joelhandwell/ubuntu_xenial64_vbguest"
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end
  config.vm.synced_folder ".", "/vagrant", disabled: true
  #config.vm.synced_folder "C:/Users", "/c/Users"
  config.vm.network "private_network", ip: "192.168.2.193"

  config.vm.provision "shell", inline: "echo install docker"
  config.vm.provision "shell", inline: "sudo apt-get install apt-transport-https ca-certificates curl software-properties-common"
  config.vm.provision "shell", inline: "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -"
  config.vm.provision "shell", inline: 'sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial edge"'
  config.vm.provision "shell", inline: "sudo apt-get update"
  config.vm.provision "shell", inline: "sudo apt-get install -y linux-image-extra-$(uname -r) linux-image-extra-virtual"
  config.vm.provision "shell", inline: "sudo apt-get install -y docker-ce"

  config.vm.provision "shell", inline: "echo user vagrant do not need sudo to run docker"
  config.vm.provision "shell", inline: "sudo gpasswd -a vagrant docker"
  config.vm.provision "shell", inline: "sudo service docker restart"
  config.vm.provision "shell", inline: 'su - vagrant -c "docker ps"'

  config.vm.provision "shell", inline: "echo install docker-compose"
  config.vm.provision "shell", inline: 'sudo curl -L "https://github.com/docker/compose/releases/download/1.16.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
  config.vm.provision "shell", inline: "sudo chmod +x /usr/local/bin/docker-compose"

  config.vm.provision "shell", inline: "echo install jid"
  config.vm.provision "shell", inline: "sudo apt-get -q -y install unzip"
  config.vm.provision "shell", inline: 'sudo curl -L "https://github.com/simeji/jid/releases/download/0.7.2/jid_linux_amd64.zip" -o /tmp/jid.zip'
  config.vm.provision "shell", inline: "sudo unzip -o /tmp/jid.zip -d /usr/local/bin/"
  config.vm.provision "shell", inline: "sudo ln -s /usr/local/bin/jid_linux_amd64 /usr/local/bin/jid"

  config.vm.provision "shell", inline: "echo install ctop, htop and netdata"
  config.vm.provision "shell", inline: "sudo wget -q https://github.com/bcicen/ctop/releases/download/v0.6.1/ctop-0.6.1-linux-amd64 -O /usr/local/bin/ctop"
  config.vm.provision "shell", inline: "sudo chmod +x /usr/local/bin/ctop"
  config.vm.provision "shell", inline: "sudo apt-get install -y htop zlib1g-dev uuid-dev libmnl-dev gcc make git autoconf autoconf-archive autogen automake pkg-config curl"
  config.vm.provision "shell", inline: "git clone https://github.com/firehol/netdata.git --depth=1"
  config.vm.provision "shell", inline: 'cd netdata && echo -e "\n" | sudo ./netdata-installer.sh'
  config.vm.provision "shell", inline: "rm -rf /home/vagrant/netdata"

  # set the locale to environment vars needed by perl
  config.vm.provision "shell", inline: "echo 'LANGUAGE=en_US.UTF-8' | sudo tee -a /etc/environment"
  config.vm.provision "shell", inline: "echo 'LC_ALL=en_US.UTF-8' | sudo tee -a /etc/environment"

  # add vagrant.pub content to authorized_keys
  config.vm.provision "shell", inline: "echo 'ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA6NF8iallvQVp22WDkTkyrtvp9eWW6A8YVr+kz4TjGYe7gHzIw+niNltGEFHzD8+v1I2YJ6oXevct1YeS0o9HZyN1Q9qgCgzUFtdOKLv6IedplqoPkcmF0aYet2PkEDo3MlTBckFXPITAMzF8dJSIFo9D8HfdOV0IAdx4O7PtixWKn5y2hMNG0zQPyUecp4pzC6kivAIhyfHilFR61RGL+GPXQ2MWZWFYbAGjyiYJnAmCP3NOTd0jMZEnDkbUvxhMmBYSdETk1rRgm+R4LOzFUGaHqHDLKLX+FIPKcF96hrucXzcWyLbIbEgE98OHlnVYCzRdK8jlqm8tehUc9c9WhQ== vagrant insecure public key' >> /home/vagrant/.ssh/authorized_keys"
  config.vm.provision "shell", inline: "echo 'LC_ALL=en_US.UTF-8' | sudo tee -a /etc/environment"

  # enable swap limit support
  # https://askubuntu.com/a/417221/1493
  config.vm.provision "shell", inline: "sudo sed -ie 's/GRUB_CMDLINE_LINUX=\"\"/GRUB_CMDLINE_LINUX=\"cgroup_enable=memory swapaccount=1\"/' /etc/default/grub"
  config.vm.provision "shell", inline: "sudo update-grub"

end
