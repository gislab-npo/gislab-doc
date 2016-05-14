.. _installation-web:
 
============
Installation
============  

As it was noticed in previous subsection, very important part is represented
by :ref:`GIS.lab Web QGIS plugin <gislab-qgis-plugin>`.

This plugin is part of ``gislab-web`` source code, so all necessary files
are downloaded with cloning process. But let us stick to this order of 
things. See instructions below.

--------------------------------
Creating development environment
--------------------------------

At first, it is necessary to clone source code with Git.

.. code:: sh

   git clone https://github.com/gislab-npo/gislab-web.git

Then optionaly GIS.lab Mobile can be enabled by adding configuration variable 
to ``provision/host_vars/gislab-web`` file as ``GISLAB_CLIENT_MOBILE: yes``.

Development environment is started after running following, well known 
command from source code root directory.

.. code-block:: sh
   :emphasize-lines: 1

   $ vagrant up

   Bringing machine 'gislab-web' up with 'virtualbox' provider...
   ==> gislab-web: Clearing any previously set forwarded ports...
   ==> gislab-web: Clearing any previously set network interfaces...
   ==> gislab-web: Preparing network interfaces based on configuration...
       gislab-web: Adapter 1: nat
   ==> gislab-web: Forwarding ports...
       gislab-web: 90 => 8090 (adapter 1)
       gislab-web: 8000 => 8000 (adapter 1)
       gislab-web: 8100 => 8100 (adapter 1)
       gislab-web: 8200 => 8200 (adapter 1)
       gislab-web: 35729 => 35729 (adapter 1)
       gislab-web: 22 => 2222 (adapter 1)
   ==> gislab-web: Running 'pre-boot' VM customizations...
   ==> gislab-web: Booting VM...
   ==> gislab-web: Waiting for machine to boot. This may take a few minutes...
       gislab-web: SSH address: 127.0.0.1:2222
       gislab-web: SSH username: vagrant
       gislab-web: SSH auth method: private key
       gislab-web: Warning: Connection timeout. Retrying...
       gislab-web: Warning: Remote connection disconnect. Retrying...
   ==> gislab-web: Machine booted and ready!
   ==> gislab-web: Checking for guest additions in VM...
   ==> gislab-web: Setting hostname...
   ==> gislab-web: Mounting shared folders...
       gislab-web: /vagrant => /home/ludka/git/gislab-web
   ==> gislab-web: Machine already provisioned. Run `vagrant provision` or use the `--provision`
   ==> gislab-web: flag to force provisioning. Provisioners marked to run always will still run.

.. note:: |note| To speed up provisioning using **Apt proxy server**, set 
   ``APT_PROXY`` variable before running above command like 
   ``$ export APT_PROXY=http://192.168.99.118:3142``. 

See :ref:`Apt Cacher server <apt-cacher-service>` instructions for more details
in this matter. Thanks to that, with next installation of server it can be faster 
because software packages will have not to be downloaded again.

After, one can log in to virtual machine - still from source code root directory.

.. code-block:: sh
   :emphasize-lines: 1
   
   $ vagrant ssh

   Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.13.0-83-generic i686)
   
    * Documentation:  https://help.ubuntu.com/
   
    System information disabled due to load higher than 1.0
   
     Get cloud support with Ubuntu Advantage Cloud Guest:
       http://www.ubuntu.com/business/services/cloud
   
   
   Last login: Wed Apr 13 08:49:28 2016 from 10.0.2.2

At this moment virtual machine is launched. Development services are started
after command below.

.. code-block:: sh
   :emphasize-lines: 1

   $ /vagrant/utils/tmux-dev.sh 
   
   ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   System    check identified no issues (0 silenced).
   May 01, 2016 - 22:17:09
   Django version 1.8.9, using settings 'devproj.settings'
   Starting development server at https://0.0.0.0:8000/
   Using SSL certificate: /home/vagrant/.virtualenvs/gislab-web/local/lib/python2.7/site-packages/sslserver/certs/development.crt
   Using SSL key: /home/vagrant/.virtualenvs/gislab-web/local/lib/python2.7/site-packages/sslserver/certs/development.key
   Quit the server with CONTROL-C.
   
   ─────────────────────────────────────────────────────────────────────┬────────────────────────────────────────────────────────────────────
   sudo tail             -n 0             -f /var/log/lighttpd/access.lo│sudo tail             -n 0             -f /var/log/lighttpd/qgis-map
   g /var/log/lighttpd/error.log                                        │server.log
   vagrant@gislab-web:~$ sudo tail             -n 0             -f /var/│vagrant@gislab-web:~$ sudo tail             -n 0             -f /var
   log/lighttpd/access.log /var/log/lighttpd/error.log                  │/log/lighttpd/qgis-mapserver.log
   ==> /var/log/lighttpd/access.log <==                                 │
                                                                        │
   ==> /var/log/lighttpd/error.log <==                                  │
                                                                        │

   [developme 0:servers*                                                                                         "gislab-web" 20:17 01-May-16 

.. _gislab-qgis-plugin:

------------------
GIS.lab Web plugin
------------------

GIS.lab Web plugin builds GIS.lab web bundle from any QGIS desktop project.
It allows adding base layers, creating topics from layers list, 
setting access constraints or project expiration.

.. _gislab-qgis-plugin-logo:

.. figure:: ../img/gislab-web/gislab-qgis-plugin-logo.svg
   :align: center
   :width: 150

   GIS.lab Web QGIS plugin icon.


All installed QGIS plugins are usually located in ``.qgis/python/plugins`` 
directory.
If ``gislab-web`` repository is correctly cloned, for GIS.lab QGIS plugin
installation just symbolic link is enough. Create it from ``gislab-web`` 
source code directory.

.. code:: sh

   ln -s $(pwd)/qgis/gislab_web  ~/.qgis2/python/plugins/gislab_web

Let's continue in QGIS environment. Create ordinary QGIS project or use some
existing one. 

.. _qgis-project:

.. figure:: ../img/gislab-web/qgis-project.png
   :align: center
   :width: 750

   Some QGIS project.

Go to :menuselection:`Plugins --> Manage and install plugins` and 
in ``Installed`` tab of dialog window find **GIS.lab Web plugin**.
Activate this plugin by checking the toggle beside it, see figure
:num:`#install-gislab-plugin`.

.. _install-gislab-plugin:

.. figure:: ../img/gislab-web/install-gislab-plugin.png
   :align: center
   :width: 750

   GIS.lab Web QGIS plugin activation.

Assuming that QGIS project is saved, run GIS.lab plugin wizard as 
:menuselection:`Web --> GIS.lab Web` or just click on plugin's icon in menu bar.
Pass through ``Base layer``, ``Layers`` and ``Project`` dialog windows
and fill in required fields and settings.

.. figure:: ../img/gislab-web/gislab-plugin-base-layer.png
   :align: center
   :width: 450

.. figure:: ../img/gislab-web/gislab-plugin-layers.png
   :align: center
   :width: 450

.. figure:: ../img/gislab-web/gislab-plugin-project.png
   :align: center
   :width: 450

   GIS.lab QGIS plugin's dialogs.

.. _gislab-qgis-plugin-publish:

Workflow is nearly finished with ``Publish`` button. 

.. figure:: ../img/gislab-web/gislab-plugin-publish.png
   :align: center
   :width: 450

   Important step in GIS.lab project publishing process.
