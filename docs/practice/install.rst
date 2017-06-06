Software customization is performed on a server (master). At first log
in locally using keyboard or via SSH protocol from controlling
machine. In virtual mode use ``vagrant ssh`` command, see :ref:`login
to GIS.lab <vagrant-login>`.

Before any modification it is recommended to **backup** current
client's root and image.

.. code-block:: sh

   $ sudo tar cjf /mnt/backup/root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root
   $ sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/image-`date -I`

Commands in client root can be performed by ``gislab-client-shell``
administration command. To enter client's root in interactive mode
``-i`` must be given.

.. code-block:: sh

   $ sudo gislab-client-shell -i
   
=========================
Software (un)installation
=========================

This section shows how to easily install or uninstall new software on
GIS.lab clients. 

.. _example-remove-geany:

Example with uninstalling `Geany <https://www.geany.org/>`__ software
is shown below.

.. code-block:: sh
   :emphasize-lines: 9

   # display geany package status details
   $ dpkg -s geany
   Status: install ok installed
   ...
   # check geany version
   $ geany --version
   geany 1.27 (built on 2016-04-17 with GTK 2.24.30, GLib 2.48.0)
   # uninstall geany
   $ sudo apt-get remove geany

Another example demostrates `Vim editor <http://www.vim.org/>`__
installation process below.

.. code-block:: sh
   :emphasize-lines: 5
   
   $ dpkg -s vim
   dpkg-query: package 'vim' is not installed and no information is available
   ...
   $ sudo apt-get update
   $ sudo apt-get install vim

When all desired changes are done the client's root is exited by
``exit`` command.

.. code-block:: sh

   $ exit

Then new client's image must be generated from modified client's root
by ``gislab-client-image`` administration command.

.. code-block:: sh

   $ sudo gislab-client-image

.. note:: |note| Main panel in client Desktop layout is generated when
   user log in firstly, so changes related to panel are displayed only
   for new user.

===========================
Custom software compilation
===========================

Let's see example custom installation of **latest development GDAL
version** from source code.

At first interactive shell in GIS.lab client's root must be entered.

.. code:: sh

   $ sudo gislab-client-shell -i

Then compilation and installation of GDAL can be executed.

.. code:: sh

   $ apt-get -y install g++ subversion
   $ cd /tmp
   $ svn checkout https://svn.osgeo.org/gdal/trunk/gdal gdal
   $ cd gdal
   $ ./configure
   $ make
   $ make install

After client's ``root`` is left by ``exit`` command, then ``image`` should 
be updated by ``sudo gislab-client-image``. 
Continue with :ref:`creation <user-creation>` of new user booting with 
latest GDAL version.

.. important:: |imp| Do not forget to set ``LD_LIBRARY_PATH`` variable and 
   configure dynamic linker run-time bindings on client before running GDAL 
   commands.
   
   .. code:: sh

      $ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
      $ sudo ldconfig
      $ /usr/local/bin/ogr2ogr --version
      GDAL 2.3.0dev, released 2017/99/99


   
