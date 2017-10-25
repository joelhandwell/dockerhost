

# dockerhost
Codified docker-machine via Vagrant on Windows for those who need fine-grained configurations enabling [versioning and automation](https://www.hashicorp.com/blog/the-tao-of-hashicorp/) which is [not provided via docker-machine](https://github.com/docker/machine/issues/773).

## Why?
Just using docker-machine [do not support custom ip](https://github.com/docker/machine/issues/1709), and custom cpu and/or memory is possible but user need to repeatingly type command like:

```
docker-machine create machine-vbox --virtualbox-cpu-count "8" --virtualbox-memory "8192"
```

## The way it codify:

clone this repo, and declare configrations like following:

```Vagrantfile```

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "joelhandwell/dockerhost"
  config.vm.provider "virtualbox" do |v|
    v.memory = 8192
    v.cpus = 8
  end
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "C:/Users", "/c/Users"
  config.vm.network "private_network", ip: "192.168.99.103"
end
```

```create.bat```

```bat
docker-machine create --driver generic --generic-ip-address=192.168.99.103 --generic-ssh-user=vagrant --generic-ssh-key=vagrant.pem default
```

and run:

```
vagrant up
create
```

## What's inside:

* Virtualbox Guest Addition 5.1.26
* docker 17.10.0-ce
* docker-compose 1.16.1
* htop 2.0.1
* [ctop](https://github.com/bcicen/ctop) 0.6.1
* [netdata](https://github.com/firehol/netdata) 1.8.0
* [jid](https://github.com/simeji/jid) 0.7.2
