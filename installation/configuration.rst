************
How to start
************

.. _install_requirements:

============================
Installation of requirements
============================

.. rubric:: GIS.lab source code download

Grab the GIS.lab *latest release* from
`https://github.com/gislab-npo/gislab/releases
<https://github.com/gislab-npo/gislab/releases>`_ and unpacking it
into a working directory.
   
.. note:: |note| One can get GIS.lab source code also by using
   Git. The instructions below are valid for Debian/Ubuntu operating
   systems.

   .. code:: sh

      $ sudo apt install git  
      $ git clone https://github.com/gislab-npo/gislab.git
     
.. _ansible-installation:

.. rubric:: Ansible installation

Ansible is an automation engine used by GIS.lab for fully automatized
provisioning. Its installation can be performed by typing ordinary
commands.

.. code-block:: sh

   $ sudo apt install ansible

.. tip::
         
  |tip| Most recent version of Ansible software can be also installed
  also by PIP.

   .. code-block:: sh

      $ sudo pip3 install ansible

  Or alternatively custom PPA can be used.

   .. code-block:: sh

      $ sudo apt install software-properties-common
      $ sudo apt-add-repository ppa:ansible/ansible
      $ sudo apt update
      $ sudo apt install ansible

--------------------------------------------------------
Additional installation of requirements for Virtual Mode
--------------------------------------------------------

These packages are needed only for installation in :doc:`virtual`.

.. _vb-installation:

.. rubric::  VirtualBox installation

Install Dynamic Kernel Module Support Framework and VirtualBox
packages. Alternatively the latest version can be downloaded from
`virtualbox.org <https://www.virtualbox.org/wiki/Downloads>`__.

.. code-block:: sh
   
   $ sudo apt install dkms virtualbox virtualbox-qt

.. _vagrant-installation:

.. rubric:: Vagrant installation

Installing vagrant package from default repositories should be
normally sufficient. Alternatively the latest version can be
downloaded from `vagrantup.com
<https://www.vagrantup.com/downloads.html>`__.

.. code-block:: sh

   $ sudo apt install vagrant

Also Vagrant `disksize plugin
<https://github.com/sprotheroe/vagrant-disksize>`__ is required and
must be installed.

.. code-block:: sh

   $ vagrant plugin install vagrant-disksize

.. tip:: If plugin installation fails, try to install more recent
   version of Vagrant.
	 
.. _configuration-section:

=============
Configuration
=============


It is recommended to set at least some basic configuration before
GIS.lab installation is performed. 

GIS.lab is designed to install and run out of box with default
configuration. However, it is required to change at least default network
configuration variable ``GISLAB_NETWORK``, if GIS.lab's default network
range ``192.168.50.0/24`` already exists in LAN to prevent IP conflicts.

Default GIS.lab configuration file named :file:`all` exists in
:file:`system/group_vars` directory located in GIS.lab source code,
see :numref:`configuration-files`.  When user decides to adjust it, this
file should not be modified directly. Instead a custom configuration
file in :file:`system/host_vars` directory should be created.

.. tip:: |tip| Find the :file:`system/group_vars/all` file in GIS.lab
   source code tree and see its content to become acquainted with all
   possibilities of configuration settings.  It is full of commented
   out information.

For installation in :doc:`Virtual mode <virtual>` it is recommended to
create file named ``gislab_vagrant`` in ``system/host_vars`` directory
for host specific GIS.lab configuration and put various changes there.

When :doc:`Physical mode <physical>` is used, file in
``system/host_vars`` directory should be named according to name of
GIS.lab unit. This name is a part of Ansible inventory file content,
script that Ansible uses to determine what to provide. All file names
must always match unique host name specified in inventory file.

.. _configuration-files:

.. figure:: ../img/installation/configuration-files.svg
   :align: center
   :width: 450

   File layout related to configuration.

File ``gislab_vagrant`` will be loaded automatically by Vagrant
without need to manually :ref:`create the Ansible inventory file
<create-ansible-inventory-file>`.

.. tip:: |tip| See :ref:`practical example <example-configuration>` of 
         configuration file.

.. seealso:: |see| :ref:`Network configuration <network-configuration>`
