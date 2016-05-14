.. _practice:
 
*******************
GIS.lab in practice
*******************

Now let's see some practical examples.

===============
GIS.lab project
===============

GIS.lab projects are created and managed by **QGIS** application, which 
is a main tool for all geospatial tasks. GIS.lab is containing its
own version of QGIS, which is improved with bug fixes and features and
it is accessible under **GIS.lab Desktop** item in GIS.lab applications menu.

**GIS.lab Web** client is automatically publishing all GIS projects
created in desktop to web environment.

Following steps will create simplest possible GIS project and will
publish it on web.

1. Log in to GIS.lab session 

Use login and password for user account that has been 
:ref:`created <user-creation>` by ``sudo gislab-adduser`` command from GIS.lab
server. 

2. Prepare data

Create working directory called ``my-first-project`` in ``~/Projects`` directory.
Copy example SpatiaLite database file 
``~/Repository/gislab-project/prague/prague.sqlite`` to 
``~/Projects/my-first-project`` directory.

3. Create project

Launch **GIS.lab Desktop** as :menuselection:`GIS.lab --> GIS.lab Desktop` 
(QGIS) and add SpatiaLite database file as 
:menuselection:`Layer --> Add SpatiaLite layer --> New`. 
Coose the database in ``Projects`` directory and connect to database by 
pressing ``Connect`` button.

Load some layer by mouse selection and press ``Add`` button. Set project 
title in :menuselection:`Project --> Project Properties --> Project title`, 
save the project 
as ``~/Projects/my-first-project/my-first-project.qgs`` with 
:menuselection:`Project > Save` option. Now first GIS project is ready.

.. _practice-gislab-web-publishing:

4. Publish project on web

Install **GIS.lab Web plugin** in 
:menuselection:`Plugins --> Manage and Install Plugins` section and launch it. 

.. note:: |note| It is safe to ignore on-the-fly transformation warning.

Publish project by pressing ``Publish`` button in plugin's wizard. 
In next step copy whole directory 
``~/Projects/my-first-project`` to ``~/Publish/<name-of-user>`` directory.

Launch **GIS.lab Web** as :menuselection:`GIS.lab --> GIS.lab Web` applications 
menu from main
GIS.lab panel. Ignore security warnings produced by self-signed certificate, 
i.e. :menuselection:`I Understand the Risks --> Add Exception --> Confirm Security Exception`
and log in with user's credentials. Then inspect published project which 
should be listed as second, right below 
default ``Empty`` project. Click on project's link in URL column to launch
project in web environment.

.. tip:: |tip| To get more familiar with possible project configurations, 
   copy some of whole GIS.lab example projects directories located in 
   ``~/Repository/gislab-project`` to ``~/Projects`` directory and start 
   exploring.

.. _example-gdal:

=====================================
Latest GDAL version on GIS.lab client
=====================================

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

.. _example-remove-geany:

===============================
Software uninstallation - Geany
===============================

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

==================================
Software installation - Vim editor 
==================================

See :ref:`software uninstallation <example-remove-geany>` section and in 
client's root enter following code. 

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
