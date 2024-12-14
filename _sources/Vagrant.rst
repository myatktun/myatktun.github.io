=======
Vagrant
=======

* to create isolated and portable development environments

Vagrantfile
-----------
    * ``config.vm.box``: Operating System
    * ``config.vm.provider``: virtualbox, vmware
    * ``config.vm.network``: how the host sees the box
    * ``config.vm.synced_folder``: to access files from local
    * ``config.vm.provision``: setup the environment

* ``vagrant up``: start the machine from Vagrantfile
* ``vagrant destroy``: delete the machine
* ``vagrant suspend``: save state and hibernate the machine
* ``vagrant resume``: restore the suspended machine
* ``vagrant halt``: power off the machine
* ``vagrant reload``: reload the machine with new settings
