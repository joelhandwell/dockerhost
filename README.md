# dockerhost
Docker host for docker-machine with flexible VM configuration via Vagrant on Windows

Why not just docker-machine?

## with docker-machine only:
```
joel@DESKTOP-90SRLRU D:\dockerhost
> docker-machine create machine-vbox
Running pre-create checks...
(machine-vbox) Image cache directory does not exist, creating it at C:\Users\joel\.docker\machine\cache...
(machine-vbox) No default Boot2Docker ISO found locally, downloading the latest release...
(machine-vbox) Latest release for github.com/boot2docker/boot2docker is v17.03.0-ce
(machine-vbox) Downloading C:\Users\joel\.docker\machine\cache\boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v17.03.0-ce/boot2docker.iso...
(machine-vbox) 0%....10%....20%....30%....40%....50%....60%....70%....80%....90%....100%
Creating machine...
(machine-vbox) Copying C:\Users\joel\.docker\machine\cache\boot2docker.iso to C:\Users\joel\.docker\machine\machines\machine-vbox\boot2docker.iso...
(machine-vbox) Creating VirtualBox VM...
(machine-vbox) Creating SSH key...
(machine-vbox) Starting the VM...
(machine-vbox) Check network to re-create if needed...
(machine-vbox) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe env machine-vbox
```
What we get:
```
docker@machine-vbox:~$ docker info
...
CPUs: 1
Total Memory: 995.8 MiB
...
```
plus, [we can not chose IP](https://github.com/docker/machine/issues/1709).


## with this:
Vagrantfile
```ruby
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
```

```
joel@DESKTOP-90SRLRU D:\dockerhost
> vagrant up
{:guestpath=>"/vagrant", :hostpath=>".", :disabled=>false}
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'AlbanMontaigu/boot2docker'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'AlbanMontaigu/boot2docker' is up to date...
==> default: Setting the name of the VM: dockerhost_default_1489771512803_65065
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 2375 (guest) => 2375 (host) (adapter 1)
    default: 2376 (guest) => 2376 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: docker
    default: SSH auth method: private key
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /c/Users => C:/Users
```
create.bat
```bat
docker-machine create --driver generic --generic-ip-address=192.168.99.103 --generic-ssh-user=docker --generic-ssh-key=vagrant.pem default
```

```
joel@DESKTOP-90SRLRU D:\dockerhost
> create

docker-machine create --driver generic --generic-ip-address=192.168.99.103 --generic-ssh-user=docker --generic-ssh-key=vagrant.pem default
Running pre-create checks...
Creating machine...
(default) Importing SSH key...
(default) Couldn't copy SSH public key : unable to copy ssh key: open vagrant.pem.pub: The system cannot find the file specified.
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: C:\ProgramData\chocolatey\lib\docker-machine\bin\docker-machine.exe env default
```
What we get:
```
docker@default:~$ docker info
...
CPUs: 8
Total Memory: 7.789 GiB
...
```
plus, we can chose IP, the box comes with latest version of Virtualbox Guest Addition, and one more thing, latest [ctop](https://github.com/bcicen/ctop).
