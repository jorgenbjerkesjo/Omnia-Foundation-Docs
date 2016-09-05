Omnia Extension Package
============================

Omnia Foundation is built with extensibility in mind since day one and an Omnia Extension Package is the main way to extend Omnia. This article will give an overview of what you can achieve with extension packages and the basic structure of an extension package. For a more practical guide on how to start developing Omnia extensions see the `Getting Started </getting-started.html>`_ guide.

What is an Omnia Extension Package?
##################################################

An Omnia extension package is a pack of features or extensions for Omnia. You can think of it like SharePoint solution packages but for the Omnia platform. An extension can be used to deploy all kinds of new features, from small UI changes to non-trivia applications with its own back-end and database. Examples are `Omnia Intranet <#>`_ and `Omnia Documentation Management <#>`_.

.. image:: /images/extensionpackage.png

At the core of an extension package are resource files. Resource files can be anything from client-side code like JavaScript, CSS, HTML templates to SharePoint artifacts like page layouts and web parts. These resources are `mapped and deployed </fundamentals/resource-mappings.html>`_ by logical containers called :doc:`Omnia feature </fundamentals/omnia-feature>`.

For example, the resources in a large extension might looks like this

.. image:: /images/extensionpackage-tenantresources.png

An extension package can also contain `Omnia jobs </fundamentals/omnia-job.html>`_, pieces of code that will run as scheduled tasks in Omnia, and `provisioning modules <#>`_, pieces of code that will be hooked into the site provisioning pipeline of Omnia. 

Manage extension packages
##################################################

All extension packages in a tenant can be managed in Omnia admin, **System > Extensions** 

.. image:: /images/omnia-admin-new-extension-upload.png

New extensions can be uploaded directly from this UI by drag and drop or from Visual Studio using Omnia Tooling. An extension need to be registered before it can be uploaded, see the `Getting Started </getting-started.html>`_ guide for more details.

In this UI you can also manage configurations for each extension by clicking on the extension name

.. image:: /images/extensionpackage-configurations.png

When a new version of an extension package has been deployed, all the new changes will not be available for users until the features of that extension are upgraded to the latest version.
