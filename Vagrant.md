## Vagrant

#### Set up a virtual development environment

Create a folder where you want to place your virtual machine and run `vagrant init`.

```
$ mkdir vagrant_getting_started
$ cd vagrant_getting_started
$ vagrant init
```

After running `vagrant init`, a `Vagrantfile` will be automatically generated in the current folder.

After we have vagrant initiated, it's time to add a box. Think of a box here as an individual development environment.

```
vagrant box add hashicorp/precise64
```

After installing the box you would like to use, you need to edit the `Vagrantfile` to set it as a base box.

```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
end
```

Now we can boot up our Vagrant environment by running `vagrant up` and get into the virtual machine using `vagrant ssh`.

We can log out the SSH session by using `CTRL + D` at any time.

To shut down the virtual machine at the end of the day, simply type `vagrant suspend`, `vagrant halt` or `vagrant destroy` to finish your job.

#### Synced folders

By default, Vagrant shares your project directory (the one with the `Vagrantfile`) to the `/vagrant` directory in your virtual machine.

#### Provision your virtual machine

Now you have your virtual machine but it needs to be provisioned.

Create a `bootstrap.sh` in the same directory as your `Vagrantfile`.

```
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www
fi
```

Then configure your `Vagrantfile` by adding the the shell script.

```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

Now if we `vagrant up` the virtual machine again (or `vagrant reload --provision` if it's already running), the apache server will be automatically installed.

