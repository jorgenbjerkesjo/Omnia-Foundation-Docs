Configurations
============================

The Configurations API allows you to read, store, update and delete configurations for you solution.

Use Configurations as a means for allowing certain values in your solution to be configurable, via code or from the Omnia Admin app, to make it more adaptable.

You reach the Configurations API through the following service

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Configurations();
	

.. contents:: The API contains the following methods:
  :local:
  :depth: 1

AddOrUpdateConfiguration
#######

Adds a new configuration, or updates an existing one.

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.AddOrUpdateConfiguration(Configuration configuration);
		
		
You find the **Configuration** class in the **Omnia.Foundation.Models.Configurations** namespace

You can also add or update a configuration with the following overload

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.AddOrUpdateConfiguration(string key, dynamic value, [string region = ""], [bool includedInClient = false], [bool uiEditable = false], [string permissionRoles = ""]);
	
AddOrUpdateConfigurations
########

To add / update multiple configurations at once, use the following method

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.AddOrUpdateConfigurations(IEnumerable<Configuration> configurations);

You find the **Configuration** class in the **Omnia.Foundation.Models.Configurations** namespace

DeleteConfiguration
########

Deletes an existing configuration by passing the name and region of it.

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.DeleteConfiguration(string name, string region);
		 
Optionally you can also pass in an extension id as the last parameter, to target configurations for a specific extension

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.DeleteConfiguration(string name, string region, [Guid? extensionPackageId = null]);

GetConfiguration
########

Gets a specific configuration by name and region.

.. code-block:: c#

	Configuration OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetConfiguration(string name, string region);
			
Optionally you can also pass in an extension id as the last parameter, to target configurations for a specific extension

 .. code-block:: c#

	Configuration OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetConfiguration(string name, string region, Guid? extensionPackageId = null]);

GetConfigurations
########

**To get all existing configurations, use the following method**

.. code-block:: c#

	IEnumerable<Configuration> OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetConfigurations();

You can also scope this to only get configurations for a specific extension by supplying the Id of the solution

.. code-block:: c#

	IEnumerable<Configuration> OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetConfigurations([Guid? extensionPackageId = null]);

**To specify which configurations to get, use the following method**

.. code-block:: c#

	IEnumerable<Configuration> OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetConfigurations(List<string> names, string region);
			
Where you pass in the names and region of the configurations to retrieve.


GetConfigurationsInRegion
########

To get all configurations in a given region, use the following method

.. code-block:: c#

	IEnumerable<Configuration> OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetConfigurationsInRegion(string region);


You can also scope this to only get configurations for a specific extension by supplying the Id of the solution

.. code-block:: c#

	IEnumerable<Configuration> OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetConfigurationsInRegion(string region, [Guid? extensionPackageId = null]);

GetOmniaInstanceMode
########

In some scenarios you might need know if Omnia is running in **Site collection** or **Tenant** mode. To get this information, call the following method

.. code-block:: c#

	OmniaInstanceModes OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetOmniaInstanceMode();
			
This will return a value from the enum **Omnia.Foundation.Models.Shared.OmniaInstanceModes**, either **SiteCollection** or **Tenant**

GetParentSiteConfigurations
########

Gets configurations from a specified parent site, based on the site URL and region of the configuration.

.. code-block:: c#

	IEnumerable<Configuration> OmniaApi.WorkWith(Ctx.Omnia()).Configurations()
		.GetParentSiteConfigurations(string fromSiteUrl, string region);
	
