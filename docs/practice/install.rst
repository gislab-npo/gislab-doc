.. _example-remove-geany:

=========================
Software (un)installation
=========================

Example with `Geany <https://www.geany.org/>`_ software is shown below.

.. code-block:: sh
   :emphasize-lines: 2,3,5,18,23,25

   # root and image backup
   $ sudo tar cjf /mnt/backup/root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root
   $ sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/image-`date -I`
   # enter interactive schell in client's root
   $ sudo gislab-client-shell -i
   
   # display geany package status details
   $ dpkg -s geany
   Status: install ok installed
   Priority: optional
   Section: devel
   Installed-Size: 2422
   Maintainer: ... ... ...
   # check geany version
   $ geany --version
   geany 0.21 (built on Mar 19 2012 with GTK 2.24.10, GLib 2.31.20)
   # uninstall geany
   $ sudo apt-get remove geany
   # leave client's root
   $ exit

   # build updated image 
   $ sudo gislab-client-image
   # create new user that boots without geany software installed
   $ sudo gislab-adduser -g User -l GIS.lab -m x@mail.com -p <psw> <name>
     
.. note:: |note| Main panel in client Desktop layout is generated with user
   creation proscess, so changes related to panel are displayed only for new 
   user.

See :ref:`software uninstallation <example-remove-geany>` section and
in client's root enter following code.

.. code-block:: sh
   :emphasize-lines: 2,3
   
   $ dpkg -s vim
   $ sudo apt-get update
   $ sudo apt-get install vim
   $ vim test
   $ a
   $ Hello VIM!
   $ :wq
   $ cat test
   Hello VIM!
   $ exit

===========================
Custom software compilation
===========================

It is assumed that GIS.lab server is running either using keyboard, monitor 
and GIS.lab server username and password, or using ``ssh key``  and 
``IP address`` together with laptop or computer which ``ssh key`` is 
registered in ``./ssh/authorized_keys`` file.
In virtual mode GIS.lab server is running after ``vagrant ssh`` command, see 
:ref:`login to GIS.lab <vagrant-login>` via ``SSH`` section.

Let's see example custom installation of **latest GDAL version** from source code.

At first client's ``root`` and ``image`` backup is recommended. In a next step
interactive shell in GIS.lab client's ``root`` should be entered.

.. code:: sh

   $ sudo tar cjf /mnt/backup/root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root
   $ sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/image-`date -I`
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
      GDAL 2.0.0dev, released 2014/04/16

   
