======================================
GIS project publication using Gisquick
======================================

GIS.lab projects are created and managed by **QGIS** application, which 
is a main tool for all geospatial tasks. GIS.lab is containing its
own version of QGIS, which is improved with bug fixes and features and
it is accessible under **GIS.lab Desktop** item in GIS.lab applications menu.

**Gisquick** client is automatically publishing all GIS projects
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

Install **Gisquick plugin** in 
:menuselection:`Plugins --> Manage and Install Plugins` section and launch it. 

.. note:: |note| It is safe to ignore on-the-fly transformation warning.

Publish project by pressing ``Publish`` button in plugin's wizard. 
In next step copy whole directory 
``~/Projects/my-first-project`` to ``~/Publish/<name-of-user>`` directory.

Launch **Gisquick** as :menuselection:`GIS.lab --> Gisquick` applications 
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

