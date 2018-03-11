.. _example-configuration:

===============================
 Example of configuration file
===============================

This section shows example of configuration used for GIS.lab master
provision, see related sections :ref:`virtual
<virtual-master-install>` and :ref:`physical <unit-installation>`
installation.

The name of file determines machine name for master. In the case of
virtual mode, the name of file should be ``gislab_vagrant`` or other
when more master virtual machines are provisioned. In physical mode,
the name of file will be probably more generic
``<name-of-gislab-unit>``, eg. ``gislab-my-organization``. The file
must be placed in :file:`system/host_vars` directory located in
GIS.lab code tree. See :ref:`configuration-section` section for
details.

Let's see practical example of configuration with some changes related
to GIS.lab network and client keyboards.

.. code-block:: sh
   :emphasize-lines: 5, 17

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

Variables ``GISLAB_NETWORK`` and ``GISLAB_CLIENT_KEYBOARDS`` in
``gislab_vagrant`` file will be different. Results after the
successful installation for both cases is demonstrated in
:numref:`config-virtual`.

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

Fourth number of server's IP address will always be ``5``. In our case
client's IP address terminates with ``50``. For left case of
:numref:`config-virtual` these addresses would look like
``192.168.50.5`` and ``192.168.50.50``, for right case
``192.168.30.5`` and ``192.168.30.50``

.. note:: |note| This information is useful in manual GIS.lab server selection  
          using :ref:`HTTP boot <http-boot-virtual>` when server's IP address is required.

.. _example-gdal:

