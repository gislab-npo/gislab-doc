.. _configuration-section:
 
*************
How to start
*************

=============================
Installation of requirements
=============================

.. _git-installation:

.. rubric:: Git installation

By far the easiest way of getting Git installed and ready to use is by using 
default repositories. This is the fastest method, but the version may 
be older than the newest version. For GIS.lab version from official repositories 
should be normally sufficient. At firt, ``apt`` package management tools can be 
used to update local package index. Afterwards, Git can be downloaded and installed.

.. code:: sh

   $ sudo apt-get update
   $ sudo apt-get install git

.. _GL-clone:

.. rubric:: GIS.lab source code download

Following command will grab GIS.lab source code to user system.

.. code:: sh

   $ git clone https://github.com/gislab-npo/gislab.git

.. _ansible-installation:

.. rubric:: Ansible installation

Ansible is an automation engine and its installation includes adding Ansible 
repository and installing it by typing ordinary commands.

.. code-block:: sh

   $ sudo apt-get install software-properties-common
   $ sudo apt-add-repository ppa:ansible/ansible
   $ sudo apt-get update
   $ sudo apt-get install ansible

.. tip:: |tip| Run following code to get **Ansible 1.9.3**

   .. code:: sh

      $ sudo apt-get install python-pip python-dev
      $ sudo pip install ansible==1.9.3

.. _vb-installation:

.. rubric::  VirtualBox installation

Firstly, add VirtualBox repository signing key, then add repository to system, 
install Dynamic Kernel Module Support Framework and finally install VirtualBox.

.. code-block:: sh
   
   $ wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
   $ sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian trusty contrib" > /etc/apt/sources.list.d/virtualbox.list'
   $ sudo apt-get update && sudo apt-get install dkms
   $ sudo apt-get install virtualbox-4.3

.. _vagrant-installation:

.. rubric:: Vagrant installation

It should be first removed previously downloaded Vagrant packages, then 
downloaded from `www.vagrantup.com <http://www.vagrantup.com/downloads.html>`_ 
and eventually package should be installed. See instructions below.

.. code-block:: sh

   $ rm -vf ~/Downloads/vagrant_*.deb
   $ sudo dpkg -i ~/Downloads/vagrant_*.deb
   $ sudo apt-get -f install

.. attention:: |att| If running 32-bit host operating system, run following command 
   to download 32-bit Vagrant box from whatever directory.
   
   .. code:: sh
   
      $ vagrant box add precise-canonical http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-i386-vagrant-disk1.box

===============
Configuration
===============


It is recommended to set at least some basic configuration before
GIS.lab installation is performed. 

GIS.lab is designed to install and run out of box with default
configuration. However, it is required to change at least default network
configuration variable ``GISLAB_NETWORK``, if GIS.lab's default network
range ``192.168.50.0/24`` already exists in LAN to prevent IP conflicts.

Default GIS.lab configuration file named ``all`` exists in ``system/group_vars``,
see figure :num:`#configuration-files`.
When user decides to adjust it, this file should not be modified directly. 

.. tip:: |tip| Find that file in GIS.lab repository and see its content to 
   become acquainted with all possibilities of configuration settings. 
   It is full of commented out information. 

For installation in VirtualBox it is recommended to create file
named ``gislab_vagrant`` in ``system/host_vars`` directory for host specific 
GIS.lab configuration and put various changes there. 

When Physical mode is used, file in ``system/group_vars`` should
be named according to name of GIS.lab unit. This name is a part 
of Ansible inventory file content, script that Ansible uses
to determine what to provide. All file names must always match unique 
host name specified in inventory file.

.. _configuration-files:

.. figure:: ../img/installation/configuration-files.svg
   :align: center
   :width: 450

   File layout related to configuration.

File ``gislab_vagrant`` will be loaded automatically by Vagrant 
without need to manually :ref:`create the Ansible inventory file <create-ansible-inventory-file>`. 
Example configuration in ``gislab_vagrant`` or ``<name-of-gislab-unit>``
file is shown below.

.. code-block:: sh
   :emphasize-lines: 5

   GISLAB_ADMIN_FIRST_NAME: Ludmila
   GISLAB_ADMIN_SURNAME: Furtkevicova
   GISLAB_ADMIN_EMAIL: ludmilafurtkevicov@gmail.com

   GISLAB_NETWORK: 192.168.50
   GISLAB_TIMEZONE: Europe/Rome
   GISLAB_DNS_SERVERS:
    - 10.234.10.10
    - 8.8.8.8
   
   GISLAB_CLIENT_ARCHITECTURE: amd64
   GISLAB_CLIENT_LANGUAGES:
    - en
    - sk
    - it
   
   GISLAB_CLIENT_KEYBOARDS:
     - layout: en
       variant: qwerty
     - layout: sk
       variant: qwerty
     - layout: it
       variant: qwerty
   
   GISLAB_CLIENT_OWS_WORKER_MIN_MEMORY: 4000

Let's see practical example of configuration with 
some changes related to GIS.lab network and client keyboards in virtual mode.
Variables ``GISLAB_NETWORK`` and ``GISLAB_CLIENT_KEYBOARDS`` in ``gislab_vagrant``
file will be different. Results after the successful installation for both cases 
are in figure :num:`#config-virtual`.

.. tip:: |tip| See :ref:`Installation in Virtual Mode <installation-virtual>`
   section for more details about the steps or just use ``vagrant provision``
   command which is used to install and configure the machine Vagrant is managing .

.. code:: sh

   file gislab_vagrant 'A'                        file gislab_vagrant 'B'
   -----------------------                        ----------------------- 
   GISLAB_NETWORK: 192.168.50                     GISLAB_NETWORK: 192.168.30
                                 
   GISLAB_CLIENT_KEYBOARDS:                       GISLAB_CLIENT_KEYBOARDS:
   - layout: sk                                   - layout: it
     variant: qwerty                                variant: qwerty

.. _config-virtual:

.. figure:: ../img/installation/config_virtual.png
   :align: center
   :width: 750

   Two different results using different Vagrant configuration file.

Fourth number of server's IP address will always be ``5`` and the first client's 
IP address will always terminate with ``50``. For left case of figure :num:`#config-virtual` 
these addresses would look like ``192.168.50.5`` and ``192.168.50.50``.

.. note:: |note| This information is useful in manual GIS.lab server selection  
          using :ref:`HTTP boot <http-boot-virtual>` when server's IP address is required.

=====================
Network configuration
=====================

This section tries to collect documentation to some of the most common
network configurations used for GIS.lab deployment. We assume, that in
all cases, machines are connected to Ethernet network with ``Gigabit switch`` 
and at least ``CAT5 e`` Ethernet cables.

------------
Virtual Mode
------------

This part of documentation assumes that GIS.lab server is installed on **Linux** 
laptop in **VirtualBox** virtual machine using **Vagrant** as it is documented in 
:ref:`Virtual Mode <installation-virtual>` installation section.

.. rubric:: Existing LAN with DHCP server

GIS.lab is deployed in existing LAN ``192.168.1.0/24`` which already
contains DHCP server and many non GIS.lab machines and network is
connected to Internet.

*Configuration*

* Laptop - wired adapter: automatic IP address assignment (Network Manager)
* Laptop - wireless adapter: disabled (Network Manager)
* ``GISLAB_NETWORK``: ``192.168.50``

.. rubric:: Separate network

GIS.lab is deployed in separate network, specially created by GIS.lab
vendor, where only GIS.lab machines are connected. Internet access is
provided by host laptop's WiFi connection and it is connected to GIS.lab
network via Ethernet cable. Network contains only GIS.lab machines.

*Configuration*

* Laptop - wired adapter: static IP address ``192.168.5.1``, mask 
  ``255.255.255.0``, gateway ``0.0.0.0``, DNS ``8.8.8.8`` (Network Manager)
* Laptop - wireless adapter: connected to Internet (Network Manager)
* ``GISLAB_NETWORK``: ``192.168.50``

-------------
Physical Mode
-------------

This section assumes that GIS.lab Unit machine is installed as it is 
documented in `Physical Mode <installation-physical>`_ installation part.

.. rubric:: Existing LAN with DHCP server

GIS.lab Unit is deployed in existing LAN ``192.168.1.0/24`` which already
contains DHCP server and many non GIS.lab machines and network is
connected to Internet.

*Configuration* 

* ``GISLAB_NETWORK``: ``192.168.50``

.. rubric:: Separate network

GIS.lab Unit is deployed in separate network, specially created by
**GIS.lab vendor**, where only GIS.lab machines are connected. Internet
access is provided by laptop running Linux, which is connected to
Internet via WiFi and to GIS.lab network via Ethernet cable. Network
contains only GIS.lab machines.

In this case, it is required to change GIS.lab Unit's wired network
adapter configuration to **static IP address** and allow connection
forwarding on laptop.

*Configuration* 

* Laptop - wired adapter: static IP address ``192.168.5.1``, mask 
  ``255.255.255.0``, gateway ``0.0.0.0``, DNS ``8.8.8.8`` (Network Manager)
* Laptop - wireless adapter: connected to Internet (Network Manager)
* ``GISLAB_NETWORK``: ``192.168.50``
* ``GISLAB_SERVER_INTEGRATION_FALLBACK_IP_ADDRESS``: ``192.168.5.5``
* ``GISLAB_SERVER_INTEGRATION_FALLBACK_GATEWAY``: ``192.168.5.1``

To allow using laptop as Internet gateway, run following commands on laptop.

.. code::

   $ sudo sysctl -w net.ipv4.ip_forward=1
   $ sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
