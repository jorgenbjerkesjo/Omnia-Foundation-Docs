File
============================

Using Omnia it is easy to provision different kinds of files to SharePoint. You can for example provision Page Layouts and Webpart Definitions, but also any other file you need.

The first thing you need to do is to add the file tou your Visual Studio Project, inside a Tenant Resources folder. Then add the file to a Resource Mappings class as :doc:`described here </fundamentals/resource-mappings>`.
Note that you need to decorate your mappings differently based on the type of file you are adding (ex. for Page Layouts).

After creating the needed Resource Mappings, follow the steps in the section below matching your file type.

.. toctree::
   :titlesonly:

   page-layout
   webpart
   generic-file
