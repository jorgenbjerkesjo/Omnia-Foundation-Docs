Omnia API
============================

Omnia contains various API's designed to simplify various tasks.

The API's are located in the **OmniaApi** class in the **Omnia.Foundation.Extensibility.Core** namespace.

The basic C# code needed to begin working with the API's from an :doc:`Omnia Feature </fundamentals/omnia-feature>` or a :doc:`Custom Web API for Omnia extensions </custom-webapi/index>` looks like below.

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia());
	
By adding a **.** after **WorkWith(Ctx.Omnia())** you will reach the available API services listed below. E.g.

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Caching();


Services
------

.. toctree::
    :titlesonly:

    caching  
    email