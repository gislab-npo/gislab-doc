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

.. _configuration-section:

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

.. tip:: |tip| See :ref:`practical example <example-configuration>` of 
         configuration file.

.. seealso:: |see| :ref:`Network configuration <network-configuration>`
