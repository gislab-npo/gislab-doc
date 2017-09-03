**********************
GIS.lab administration
**********************

.. _user_accounts:

========================
User accounts management
========================

.. _user-creation:

-----------------
Creating new user
-----------------

New user accounts can be created by using ``gislab-adduser`` command,
the command below creates ordinary user `lab1` with `lab` as password.

.. code:: sh 

   $ sudo gislab-adduser -g User -l GIS.lab -m lab1@gis.lab -p lab lab1

.. note:: |note| Superuser accounts can be created by ``-s`` flag,
   such user will be able to perform ``sudo`` operations on client
   machines.

.. tip:: |tip| See :ref:`user-customization` section to perform
         customization when creating or deleting user accounts.
         
-------------------
List existing users
-------------------

With ``gislab-listusers`` list of all GIS.lab users is displayed, see
example below.

.. code:: sh

   $ sudo gislab-listusers | grep uid:
   uid: uid=gislab
   uid: uid=lab1

===================
Machines management
===================

.. _client-enabling:

-----------------------
Enabling GIS.lab client
-----------------------

By default, no client machines are allowed to boot from GISlab
server. To allow client machine, there are similar steps to steps
described for :ref:`virtual <client-enabling-virtual>` mode. Simply
run ``gislab-machines`` command on **GIS.lab server** and enable the
client.

.. code:: sh

   sudo gislab-machines -a <MAC-address>

.. tip:: Good way to collect ``MAC addresses`` of client machines is
   to plainly let them try to boot and than run following command to
   get list of denied MAC addresses on GIS.lab server.

   .. code:: sh

      $ sudo grep -e 'DHCPDISCOVER.*no free leases' /var/log/syslog 

.. _network-management:

==================
Network management
==================

GIS.lab network can operate in two modes. GIS.lab is possible to
integrate into existing (corporate) local area network (LAN) or to run
its own computer network controlled by DHCP server on GIS.lab master
machine. By default (since GIS.lab version 0.6) DHCP service is
**disabled** on master. GIS.lab network is managed by
``gislab-network`` administration command.

.. note:: This functionality has been added in GIS.lab version 0.6.
          
Current status is reported by

.. code:: sh
             
   $ sudo gislab-network status

::

   [GIS.lab]: Connection forwarding service is disabled and inactive.
   [GIS.lab]: DNS service is disabled and inactive.
   [GIS.lab]: DHCP service is disabled and inactive.

DHCP and DNS service can be started on master node by

.. code:: sh
             
   $ sudo gislab-network start

This settings is not persistent, to enable DHCP/DNS service
automatically after booting master run:

.. code:: sh
             
   $ sudo gislab-network enable
