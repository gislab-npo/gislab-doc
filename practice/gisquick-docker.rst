===========================
Gisquick Docker integration
===========================

Gisquick is intergrated to GIS.lab environment as *Docker
service*. Gisquick application is split into 3 services running in
Docker containers:

* QGIS server
* Django Application served with Gunicorn
* Nginx Server

See more information in `Gisquick documentation
<http://gisquick.readthedocs.io/en/latest/administrator-manual/installation/docker.html>`__.

Gisquick docker files are located on GIS.lab master node in
:file:`/opt/gislab/system/docker/gisquick/` folder. GIS.lab comes with
modified Django image with LDAP support and HTTP as default protocol.

Gisquick service can managed from
:file:`/opt/gislab/system/docker/gisquick/` folder on GIS.lab master
node by standard ``docker-compose`` command, see `Gisquick
documentation
<http://gisquick.readthedocs.io/en/latest/administrator-manual/installation/docker.html#update-installation>`__
for details.
