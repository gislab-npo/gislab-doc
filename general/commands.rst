.. _commands:
 
***************
Useful commands
***************

``gislab-adduser``
   creates GIS.lab user account

``gislab-backupall``
   backups all GIS.lab user accounts

``gislab-backupclient``
   backups GIS.lab client root and image

``gislab-backupuser``
   backups GIS.lab user account

.. _gislab-client-image:

``gislab-client-image``
   builds new GIS.lab client image

.. _gislab-client-shell:

``gislab-client-shell``
   runs command or launches interactive shell in GIS.lab client's root

``gislab-cluster event <event>``
   sends a custom event through the Serf cluster

``gislab-cluster event reboot``
   reboot all members of cluster

``gislab-cluster event reboot <hostname>``
   reboot particular client machines

``gislab-cluster event shutdown``
   shutdown all members of cluster

``gislab-cluster leave``
   gracefully leaves the Serf cluster and shuts down 

``gislab-cluster members``
   lists the members of a serve cluster

``gislab-cluster members -tag sesion-active=*``
   lists client machines which are currently running user session

``gislab-deluser``
   removes GIS.lab user account

``gislab-listusers``
   lists GIS.lab users

``gislab-machines``
   adds or removes GIS.lab client machine's MAC address

``gislab-password``
   changes GIS.lab user's password

``gislab-restoreuser``
   restores GIS.lab user account from backup

``nslookup``
   displays information that can be used to diagnose DNS infrastructure, e.g. 
   domain name or IP address, it is available only if 
   TCP/IP protocol is installed

``ping <hostname-or-ip-address-of-the-target-computer>``
   sends a test packet of data to a designated IP address to test connection 
   using the TCP/IP protocol, it finds out whether the peer host/gateway is 
   reachable

``shutdown -h now``
   brings the system down; instructs the hardware to stop all CPU functions
   immediately 

``vagrant destroy`` 
   stops the running Vagrant machine and destroys all resources that were 
   created during the machine creation process

.. _vagrant-halt:

``vagrant halt``
   shuts down the running machine Vagrant is managing

.. _vagrant-provision:

``vagrant provision``
   runs any configured provisioners that allow user to automatically install 
   software, alter configurations, and more on the machine as part of the 
   :ref:`vagrant up <vagrant-up>` process against the running Vagrant managed 
   machine

``vagrant provision --provision-with test``
   runs tests with Vagrant

.. important:: |imp| Variable ``GISLAB_TESTS_ENABLE`` must be set as ``yes`` 
   in ``system/host_vars/gislab_vagrant`` file.

``vagrant reload`` 
   the equivalent of running :ref:`vagrant halt <vagrant-halt>` followed by 
   :ref:`vagrant up <vagrant-up>`

.. _vagrant-status:

``vagrant status``
   tells the state of the machines Vagrant is managing 

.. _vagrant-up:

``vagrant up``
   creates and configures guest machines according to *Vagrantfile*

.. _vagrant-version:

``vagrant version``
   tells the version of the installed Vagrant as well as the latest version of 
   Vagrant that is currently available

``VBoxManage list runningvms``
   gets a list of all running VirtualBox virtual machines
