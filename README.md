# TSEM-vagrant

This is repository contains Vagrantfile which automatically deploys a headless
Ubuntu 22.04 virtual machine in VirtualBox with TSEM enabled kernel:
https://github.com/Quixote-Project/TSEM/tree/TSEM-6.12 and corresponding Quixote
utilities: https://github.com/Quixote-Project/Quixote installed. It uses
relatively minimal config, based on make localmodconfig and adjusted to be
compilable with TSEM.

To install and set up Vagrant, refer to
https://developer.hashicorp.com/vagrant/install

After cloning this repository, launching the Vagrantfile requires only single command:

```
vagrant up
```

To ssh into the virtual machine:

```
vagrant ssh
```
