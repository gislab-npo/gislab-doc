.. _project-publishing:
 
==================
Project publishing
==================

When ``Publish`` button is successfully pressed in GIS.lab QGIS plugin
:ref:`dialog <gislab-qgis-plugin-publish>`, 
unique project file name with timestamp together with it's metafile are created.

Then it is necessary to **copy** published QGIS project with all associated data 
to ``vagrant`` directory that is located in ``gislab-web`` source code.

.. figure:: ../img/gislab-web/vagrant-directory.svg
   :align: center
   :width: 450

   Directory for QGIS projects going to be published.

.. seealso:: |see| :ref:`Source code layout <source-code-layout>`

As the final step, open web browser and launch published project in GIS.lab Web 
interface by entering URL below.
You will see welcome screen with possibility to enter credential but for now, 
you can just ``Continue as guest``. 

.. code:: 

   https://localhost:8000?PROJECT=vagrant/<project-directory-name>/<qgs-file-name>

.. _gislab-web-welcome:

.. figure:: ../img/gislab-web/gislab-web-welcome.png
   :align: center
   :width: 750

   GIS.lab Web welcome screen.

And now there are no obstacles to enjoy your published project.

.. _gislab-we-published:

.. figure:: ../img/gislab-web/gislab-web-published.png
   :align: center
   :width: 750

   QGIS project published with GIS.lab Web.

.. seealso:: |see| See :ref:`Publish project on web <practice-gislab-web-publishing>`
   section with publishing QGIS projects from GIS.lab Desktop environment.

Type ``tmux kill-session`` to destroy the given session, closing any windows 
linked to it and no other sessions, and detaching all clients attached to it.
Then use ``logout`` to log out from virtual 
machine and ``vagrant halt`` to shut down the running machine Vagrant 
is managing.

.. tip:: |tip| Use following command to run server tests from 
   ``/vagrant/dev/django`` directory.

   .. code:: sh

      $ python ./manage.py test webgis.viewer.tests

.. note:: |note| QGIS Mapserver is also forwarded to host machine on port ``8090``.
   Its logs can be found in ``/var/log/lighttpd`` directory.

