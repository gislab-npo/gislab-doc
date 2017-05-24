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
should be normally sufficient.

The instructions below are valid for Debian/Ubuntu operating
systems. At first, ``apt`` package management tools can be used to
update local package index. Afterwards, Git can be downloaded and
installed.

.. code:: sh

   $ sudo apt-get install git

.. _GL-clone:

.. rubric:: GIS.lab source code download

Following command will grab the *most recent* GIS.lab source code to user system.

.. code:: sh

   $ git clone https://github.com/gislab-npo/gislab.git

.. note:: |note| Git is not necessary but it is recommended. One can
   get GIS.lab source code also by downloading latest GIS.lab package
   from `https://github.com/gislab-npo/gislab.git
   <https://github.com/gislab-npo/gislab.git>`_ and unpacking it in
   working directory.

For production environment it's recommended to grab recent `stable
version <https://github.com/gislab-npo/gislab/releases>`__ (source
code) directly from GitHub.
      
.. _ansible-installation:

.. rubric:: Ansible installation

Ansible is an automation engine used by GIS.lab for fully automatized
provisioning. Its installation can be performed by typing ordinary
commands.

.. code-block:: sh

   $ sudo apt-get install ansible

.. attention::

    |att| Since version 0.7 GIS.lab requires Ansible version >= 2.
   
.. tip::
         
  |tip| Most recent version of Ansible software can be also installed
  also by PIP.

   .. code-block:: sh

      $ sudo pip install ansible

  Or alternatively custom PPA can be used.

   .. code-block:: sh

      $ sudo apt-get install software-properties-common
      $ sudo apt-add-repository ppa:ansible/ansible
      $ sudo apt-get update
      $ sudo apt-get install ansible
   
.. _vb-installation:

.. rubric::  VirtualBox installation

Install Dynamic Kernel Module Support Framework and VirtualBox
packages. These packages are needed only for installation in
:doc:`virtual`.

.. code-block:: sh
   
   $ sudo apt-get install dkms virtualbox

.. _vagrant-installation:

.. rubric:: Vagrant installation

Installing vagrant package from default repositories should be
normally sufficient. The latest version can be downloaded from
`vagrantup.com <http://www.vagrantup.com/downloads.html>`__. Vagrant
is required only for installation in :doc:`virtual`.

.. code-block:: sh

   $ sudo apt-get install vagrant

.. attention:: |att| If running 32-bit host operating system, run following command 
   to download 32-bit Vagrant box from whatever directory.
   
   .. code:: sh
   
      $ vagrant box add xenial-canonical \
      https://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-i386-vagrant.box

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
see :num:`#configuration-files`.  When user decides to adjust it, this
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
