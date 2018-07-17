=================
Salt Vagrant Demo
=================

A Salt Demo using Vagrant.

https://docs.saltstack.com/en/getstarted/fundamentals/index.html


Instructions
============

Run the following commands in a terminal. Git, VirtualBox and Vagrant must
already be installed.

.. code-block:: bash

    git clone https://github.com/UtahDave/salt-vagrant-demo.git
    cd salt-vagrant-demo
    vagrant plugin install vagrant-vbguest
    vagrant up


This will download an Ubuntu  VirtualBox image and create three virtual
machines for you. One will be a Salt Master named `master` and two will be Salt
Minions named `minion1` and `minion2`.  The Salt Minions will point to the Salt
Master and the Minion's keys will already be accepted. Because the keys are
pre-generated and reside in the repo, please be sure to regenerate new keys if
you use this for production purposes.

You can then run the following commands to log into the Salt Master and begin
using Salt.

.. code-block:: bash

    vagrant ssh master

    # when running salt commands on master, must be sudo
    sudo su
    
    # List all keys submitted by minions
    salt-key --list-all

    sudo salt \* test.ping

    # Run commands and states on all minions
    # (The states below are shipped with saltstack)

    # Run an arbitrary shell cmd
    salt '*' cmd.run 'ls -l /etc'
    
    # Run the disk.usage state
    salt '*' disk.usage
    
    # Run the pkg.install state with one arg, cowsay
    salt '*' pkg.install cowsay
    
    # Run the network.interfaces state
    salt '*' network.interfaces

    # different ways to target minions (servers) under control
    salt 'minion1' disk.usage
    salt 'minion*' disk.usage
    salt -G 'os:Ubuntu' test.ping
    salt -E 'minion[0-9]' test.ping
    salt -L 'minion1,minion2' test.ping

    # Apply the custom nettools state on minion2
    salt 'minion2' state.apply nettools

    # Update your top.sls file and highstate
    salt '*' state.apply
    salt 'minion1' state.apply

    # run mysql_secure_installation on minion2
    # visit http://192.168.50.11/ to see apache2 on minion1

    # Show how detailed you can make states https://github.com/AppThemes/salt-config-example
