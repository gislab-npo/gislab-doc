.. _tips:

***************
Tips and tricks
***************

.. _apt-cacher-service:

==================
Apt Cacher service
==================

Vagrant file for Apt Cacher service:

.. code:: sh

   # -*- mode: ruby -*-
   # vi: set ft=ruby :

   GISLAB_NETWORK="192.168.50"

   VAGRANTFILE_API_VERSION = "2"
   Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
     config.vm.box = "precise-canonical"

     config.vm.provider "virtualbox" do |v|
       v.customize ["modifyvm", :id, "--memory", "512"]
       v.customize ["modifyvm", :id, "--nictype1", "virtio"]
       v.customize ["modifyvm", :id, "--nictype2", "virtio"]

       config.vm.network "forwarded_port", guest: 3142, host: 3142, auto_correct: true
     end

     config.vm.hostname = "apt-cacher"
     config.vm.provision "shell", inline: "apt-get install apt-cacher-ng"
     config.vm.network "public_network", ip: "%s.%s" % [GISLAB_NETWORK, "6"]
   end

Run Apt Cacher server by typing ``vagrant up`` and add following line to 
GIS.lab configuration file:

.. code:: sh

   GISLAB_APT_HTTP_PROXY: http://192.168.50.6:3142

.. _customization-ansible:

============================================
Executing customization scripts from Ansible
============================================

.. todo:: |todo| prejsť!

Following example will execute the same script first on GIS.lab Server 
and than in GIS.lab client's ``root``. See Ansible playbook below.

.. code:: sh

   ---
   
   # Example GIS.lab customization playbook.
   
   - hosts: all
     sudo: yes
   
     vars:
       SERVER_SCRIPT: gislab-customize.sh
       CLIENT_SCRIPT: gislab-customize.sh
       GISLAB_INSTALL_CLIENTS_ROOT: /opt/gislab/system/clients
   
     tasks:
       # Customize GIS.lab Server
       - name: Run script on server
         script: "{{ SERVER_SCRIPT }}"
         tags:
           - customize-server
   
       # Customize GIS.lab Desktop client
       - name: Copy script to client's root
         copy: src={{ CLIENT_SCRIPT }}
               dest={{ GISLAB_INSTALL_CLIENTS_ROOT }}/desktop/root/tmp/customize.sh
               owner=root group=root mode=0755
         tags:
           - customize-client
   
       - name: Run script in client's root
         shell: gislab-client-shell /tmp/customize.sh
         tags:
           - customize-client
   
       - name: Remove script from client's root
         file: path={{ GISLAB_INSTALL_CLIENTS_ROOT }}/desktop/root/tmp/customize.sh
               state=absent
         tags:
           - customize-client
   
       - name: Rebuild client image
         shell: gislab-client-image
         tags:
           - customize-client
           - build-cient-image
   
   # vim:ft=ansible:

Example customization script would be as follows.

.. code:: sh

   #!/bin/bash
   # Example GIS.lab customization script.
   # Author: Ivan Mincik, ivan.mincik@gmail.com
   
   
   # detect if we are running on GIS.lab Server or inside GIS.lab Desktop
   # Client root
   if [ "$(ls -di /)" == "2 /" ]; then
       echo "Hello from GIS.lab Server."
   else
       echo "Hello from GIS.lab Client's root."
   fi
   
   
   # vim: set ts=4 sts=4 sw=4 noet:

And for running Ansible playbook in Vagrant environment see next example.

.. code:: sh

   PYTHONUNBUFFERED=1 \
   ANSIBLE_FORCE_COLOR=true \
   ANSIBLE_HOST_KEY_CHECKING=false \
   ANSIBLE_SSH_ARGS='-o UserKnownHostsFile=/dev/null -o ForwardAgent=yes -o ControlMaster=auto -o ControlPersist=60s' \
   ansible-playbook -v \
   --private-key=$(pwd)/.vagrant/machines/gislab_vagrant/virtualbox/private_key \
   --user=vagrant \
   --connection=ssh \
   --limit='gislab_vagrant' \
   --inventory-file=$(pwd)/.vagrant/provisioners/ansible/inventory \
   --tags customize-server,customize-client,build-cient-image \
   gislab-customize.yml 

.. _cluster-parallel-ssh:

===================================================
Running commands on whole cluster with parallel-ssh
===================================================

Deploy public ``ssh`` key to GIS.lab user to allow passwordless login.

.. code:: sh

   $ ssh-copy-id -i ~/.ssh/id_rsa.pub  gislab@<GIS.lab server IP>

Get list of currently running client machines

.. code:: sh

   $ MACHINES="$(gislab-cluster members -tag role=client -status=alive | awk -F " " '{printf "%s ", $1}')"

Install gedit on all client machines

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" sudo DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends gedit

   [1] 23:02:57 [SUCCESS] c51
   Reading package lists...
   Building dependency tree...
   Reading state information...
   The following NEW packages will be installed:
     gedit
   0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
   Need to get 0 B/827 kB of archives.
   After this operation, 2,781 kB of additional disk space will be used.
   Selecting previously unselected package gedit.
   (Reading database ... 134642 files and directories currently installed.)
   Unpacking gedit (from .../gedit_3.4.1-0ubuntu1_amd64.deb) ...
   Processing triggers for desktop-file-utils ...
   Setting up gedit (3.4.1-0ubuntu1) ...
   update-alternatives: using /usr/bin/gedit to provide /usr/bin/gnome-text-editor (gnome-text-editor) in auto mode.
   Processing triggers for libc-bin ...
   ldconfig deferred processing now taking place
   [2] 23:02:57 [SUCCESS] c50
   Reading package lists...
   Building dependency tree...
   Reading state information...
   The following NEW packages will be installed:
     gedit
   0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
   Need to get 0 B/827 kB of archives.
   After this operation, 2,781 kB of additional disk space will be used.
   Selecting previously unselected package gedit.
   (Reading database ... 134642 files and directories currently installed.)
   Unpacking gedit (from .../gedit_3.4.1-0ubuntu1_amd64.deb) ...
   Processing triggers for desktop-file-utils ...
   Setting up gedit (3.4.1-0ubuntu1) ...
   update-alternatives: using /usr/bin/gedit to provide /usr/bin/gnome-text-editor (gnome-text-editor) in auto mode.
   Processing triggers for libc-bin ...
   ldconfig deferred processing now taking place

Perform performance test of parallel write to network share

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/dev/zero of=/mnt/barrel/file-$(hostname).io bs=1M count=1024'

   [1] 09:42:11 [SUCCESS] c54
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 37.7824 s, 28.4 MB/s
   [2] 09:42:11 [SUCCESS] c52
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 38.1136 s, 28.2 MB/s
   [3] 09:42:11 [SUCCESS] c51
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 38.4403 s, 27.9 MB/s
   [4] 09:42:12 [SUCCESS] c53
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 38.6802 s, 27.8 MB/s

Perform performance test of parallel read from network share

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/mnt/barrel/file-$(hostname).io of=/dev/zero bs=1M'

   [1] 09:42:45 [SUCCESS] c51
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.207453 s, 5.2 GB/s
   [2] 09:42:45 [SUCCESS] c53
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.210259 s, 5.1 GB/s
   [3] 09:42:45 [SUCCESS] c52
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.227793 s, 4.7 GB/s
   [4] 09:42:45 [SUCCESS] c54
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.207774 s, 5.2 GB/s

Perform CPU performance test

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/dev/zero bs=1M count=1024 | md5sum'

   [1] 09:39:05 [SUCCESS] c52
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c52,192.168.19.52' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.51008 s, 428 MB/s
   [2] 09:39:05 [SUCCESS] c53
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c53,192.168.19.53' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.50255 s, 429 MB/s
   [3] 09:39:06 [SUCCESS] c54
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c54,192.168.19.54' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.52551 s, 425 MB/s
   [4] 09:39:06 [SUCCESS] c51
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c51,192.168.19.51' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.56706 s, 418 MB/s

.. _pxe-boot-lenovo:

=================================================
Procedure of enabling PXE boot for Lenovo machine
=================================================

Here is an example procedure of enabling PXE boot for Lenovo ThinkPad. 

Firstly, boot up computer. Press ``F2``, then press ``Enter`` and ``F1`` key. 
This should take you to the BIOS screen. Select ``Security``, ``Secure Boot``, 
set to ``Disable``, select ``Start Up``, ``UEFI/Legacy Boot``, set to
``Legacy Only`` and press ``F10``. Once you press ``F10``, reboot and then 
press ``F12``. You should now be at the boot menu. Select ``PCI LAN`` and 
press ``Enter``.

.. _pxe-boot-dell:

===============================================
Procedure of enabling PXE boot for Dell machine
===============================================

Following examples shows enabling PXE boot for Dell Precision M4400 Mobile 
Workstation.

Start with machine booting. Press ``F12``, go to ``BIOS Setup``, find
``Settings``, ``System configuration``, ``Integrated NIC`` and set 
``Enabled w/PXE``. Then press ``Exit`` button, reboot and boot from 
**Onboard NIC**.

.. _public-events-and-queries:

=========================
Public events and queries
=========================

Here is a list of publicly available events and queries designed for
ordinary usage. This list does not contain system events and queries
which are used for internal GIS.lab cluster management.

Get a list of cluster members of a Serf cluster by typing 
``gislab-cluster members``. 

.. code:: sh

   server.gis.lab  192.168.50.5:7946   alive  role=server
   c51             192.168.50.51:7946  alive  role=client

Or get this list in JSON format with ``gislab-cluster members -format json``
command.

.. code:: json

   {
     "members": [
       {
         "name": "server.gis.lab",
         "addr": "192.168.50.5:7946",
         "port": 7946,
         "tags": {
           "role": "server"
         },
         "status": "alive",
         "protocol": {
           "max": 4,
           "min": 2,
           "version": 4
         }
       },
       {
         "name": "c51",
         "addr": "192.168.50.51:7946",
         "port": 7946,
         "tags": {
           "role": "client"
         },
         "status": "alive",
         "protocol": {
           "max": 4,
           "min": 2,
           "version": 4
         }
       }
     ]
   }


For more commands see :ref:`Useful commands <commands>` section with ``<cluster>``
key word. For example command 
``gislab-cluster members -tag sesion-active=*`` lists 
client machines which are currently running user session. After GIS.lab user's 
login there will be list as follows.

.. code:: sh

   server.gis.lab  192.168.50.5:7946   alive  role=server
   c51             192.168.50.51:7946  alive  role=client,session-active=ludka

.. seealso:: |see| :ref:`Running commands on whole cluster with parallel-ssh <cluster-parallel-ssh>`

-------------------------
Remote desktop management
-------------------------

Connect to running remote desktop session using following command.

.. code:: sh

   HOST=<REMOTE-HOST-NAME> ssh gislab@$HOST "x11vnc -bg -safer -once -nopw -scale 0.9x0.9 -display :0 -allow $(hostname -f)" && vncviewer $HOST

.. _network-configuration:

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

