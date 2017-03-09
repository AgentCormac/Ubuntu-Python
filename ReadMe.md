Python
============

This is a Vagrant environment for testing/learning about Python and the [Fujtisu K5 IaaS](https://s-portal.cloud.global.fujitsu.com) API when you are handicapped by having a Windows machine. ;0)

K5 guides and documentation can be found [here](http://www.fujitsu.com/global/solutions/cloud/k5/guides/).

**Note:** K5 portal is IE only as at 20/01/2017.

The Vagrant script will spin up a CentOS 7 gui machine and install Python 2.7, Robot Framework WebDemo and all the dependencies needed to develop in Python to drive the Fujitsu K5 IaaS APIs:

**NOTE:** This is not the way to write an Ansible Playbook. It is just a quick and dirty script to get up and running quickly.

Instructions
------------

Vagrant up
* Wait for provisioning to complete
* Use GUI to login - vagrant/vagrant
* Open a terminal window

OR

* vagrant ssh python

Your development project should be placed in ~vagrant/shared/dev. This will share access to the code base from your favourite windows IDE or use/install tools on the VM (Atom editor).

Robotframework web demo is also included (as a hangover from a previous box).

```
cd webdemo
python demoapp/server.py &
robot login-tests/
```
**NOTE:** Tests can be disrupted by Firefox setup dialogues, which can happen after an update/install.
**NOTE:** Running a gui in a vm is not a good idea. May need to increase the wait time and/or memory allocated to the VM for the vm to enable the browser to respond in time for the framework.
```
${DELAY}          0 in login_tests/resource.robot
```

Pre-requisites
--------------

* Virtual Box
* Cygwin
* Vagrant (1.8.5)
* Vagrant plugin vagrant-vbguest

The Vagrantfile will spin up the VMs and install Ansible, ssh-keys etc. Ansible will the provision the VM. The ansible script can be found in the **shared** directory.

K5 API development
==================
This box was created to take advantage of the great work carried out by Nick Cross over at [mohclips](https://github.com/mohclips/k5-ansible-infra) (20/01/2017). Be sure to recusively clone the repository into shared/dev
```
git clone --recursive https://github.com/mohclips/k5-ansible-infra.git
```

Dependencies
------------

K5 API python (pip) development dependencies:


* requests
* cryptography
* shade

Linux dependencies:


* python-devel
* python-crypto
* libffi-devel
* python-netifaces
* openssl-devel

Create your own openrc credentials file and run
```
. openrc
```
See [mohclips](https://github.com/mohclips/k5-ansible-infra) for further details.

K5 API Ansible
==============

get repo from mohclips (as described above).
```
cd k5-ansible-infra
. openrc
ansible-paybook provision_infra.yml
```

Enjoy!
