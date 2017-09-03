.. _conventions:

**************************************
Conventions used in this documentation
**************************************

There are many different organizational and typographical features throughout 
this documentation designed to help you get the most out of the material.
You will find number of styles of text that distinguish 
between different kinds of information. Here are some types of headings, 
examples of typographical conventions, styles and an explanation of their 
meaning.

.. _styles-text:

==============
Styles of text
==============

.. sidebar:: Example

   .. code:: 
   
      *Italic*
      **Bold**
      :superscript:`superscript`
      :subscript:`subscript`
      ``Code text``

*Italic* indicates mainly important or key terms, URLs or email addresses.

**Bold** shows new terms and other text indicating that we wish to draw your attention.

Other roles like :superscript:`superscript` and :subscript:`subscript` text are 
displayed in this way.

``Code text`` represents code, commands, options, switches, variables, 
attributes, keys, functions, types, classes, namespaces, methods, modules, 
properties, parameters, values, directories, objects, events, 
event handlers, tags, macros, the contents of files, or the output from commands. 

More comprehensive parts are written in blocks as follows: 

.. code-block:: python
   :emphasize-lines: 3
   :linenos:

   some numbered python code
   if re.match(r'^\d{3}-\d{4}$', test_string):
   some numbered python code highlighted
   some numbered python code 

Shell commands beginning with :command:`$` (dollar) should be run in command window.

.. code-block:: sh

   $ some shell script code
   $ if ! [ $MAX_NO -ge 5 -a $MAX_NO -le 9 ] ; then
   $ some shell script code

.. code::

	<various code block>

.. sidebar:: Example

   .. code:: 

      :command:`some command` 
      :guilabel:`Guilabel`
      :menuselection:`First step --> Second step`
      :file:`file.svg`
      `GIS.lab project page <http://gislab-npo.github.io/gislab/>`_
      :num:`#some-figure-t`
      :ref:`Conventions <conventions>`

General commands are written as :command:`some command`, guilabel as
:guilabel:`Guilabel`, direction through a menu is displayed as
:menuselection:`First step --> Second step`, name of file is
represented by :file:`file.svg`. For usage of footnotes, see [#name]_,
external hyperlinks are represented as `GIS.lab web page
<http://gislab-npo.github.io/gislab/>`_, for reference to some picture, see
:num:`#some-figure-t`, :num:`#some-figure-s`, :num:`#some-figure-m`
and :num:`#some-figure-l`, for reference to some part of page, see
:ref:`Conventions <conventions>`.

*Example of useful term*
   description ... 

``Example of useful command``
   description ...

================
Short paragraphs
================

.. tip:: |tip| This signifies a tip, suggestion, or general useful note.

.. attention:: |att| This style indicates a warning or caution.

.. note:: |note| This is note.

.. important:: |imp| This represents something important.

.. danger:: |danger| This style indicates a warning or caution.

.. seealso:: |see| This note leads the user to another material that is on the similar level of scope.

.. note is displayed only if ``todo_include_todos`` in ``conf.py`` is set as ``True``.

.. todo:: |todo| This signifies some issue to be done next time.

=================
Types of Headings
=================

For style of chapter names, please :ref:`see <conventions>` chapter name above,
for example of section, :ref:`see <styles-text>` subsection above, others are 
shown below.

----------
Subsection
----------

^^^^^^^^^^^^^
Subsubsection
^^^^^^^^^^^^^

"""""""""
Paragraph
"""""""""

####
Part
####

.. rubric:: Paragraph heading 
   
etc.

.. sidebar:: Example

   .. code:: 

     .. _some-figure-t:
     
     .. figure:: ../img/login_text_logo.svg
        :align: center
        :width: 150
     
        GIS.lab unit tiny.     

=======
Figures
======= 

.. _some-figure-t:

.. figure:: ../img/login_text_logo.svg
   :align: center
   :width: 150

   GIS.lab unit tiny.

.. _some-figure-s:

.. figure:: ../img/login_text_logo.svg
   :align: center
   :width: 250

   GIS.lab unit small.

.. _some-figure-m:

.. figure:: ../img/login_text_logo.svg
   :align: center
   :width: 450

   GIS.lab unit middle.

.. _some-figure-l:

.. figure:: ../img/login_text_logo.svg
   :align: center
   :width: 750

   GIS.lab unit large.

======
Tables
======

.. csv-table:: Table with GIS.lab contributors.
   :header: "Contributors to GIS.lab documentation", "Country"
   :widths: 20, 10

   "Ludmila Furtkevicova", "Slovakia"
   "Ivan Mincik", "Slovakia"
   "Martin Landa", "Czech republic"
   "...", "..."

=====================================
Sidebars, lists and quote-like blocks
=====================================

.. sidebar:: Some Sidebar 

   ``vagrant up``

   .. code:: 

      .. sidebar:: Some Sidebar

         ``vagrant up``

   Example of lists:

   .. code:: 

      #. numbered list 

        #. nested numbered list

      * bulleted list 

        * nested bulleted list

#. numbered list 

  #. nested numbered list

* bulleted list 

  * nested bulleted list

=======
Columns
=======

Example of three columns is shown below.

.. hlist::
    :columns: 3

    * A
    * B
    * C
    * D 
    * E
    * F
    * G
    * H
    * I
    * J
    * K
    * L 

=========
Footnotes
=========

.. [#name] Some footnote.

.. seealso:: |see| `Coding conventions <https://github.com/gislab-npo/gislab/wiki/Coding-Guidelines>`_.


