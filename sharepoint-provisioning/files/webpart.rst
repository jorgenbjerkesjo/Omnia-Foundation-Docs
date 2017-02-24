Webpart Definition
============================

Deploying a webpart definition is very similar to provisioning a page layout. Web part definitions are also stored in the root web of a site collection, but in the Webpart gallery library. herefore it is recommended to provision page layout files in a Site Collection scoped feature.

In a Site Collection scope feature, locate the **public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)** method.

Add the following code, replacing **TenantResourcesMapping.Webparts** with the class name of your Webpart in the Resource Mappings class, and **MyWebPart** with the property representing your webpart definition.

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.Webparts>(q => q.MyWebPart)
			.WithSettingsForWebPart()
			.DeploysTo(SharePointFileDeploymentTargets.WebPartGallery);
	}

.. note:: This is a fluent API, see the :doc:`Page layout section <page-layout>` for more information about how to for example change the file name of the webpart definition file when it is provisioned.

Other files
####

There could be a need in your project to provision other kinds of files than the ones outlined above. This could be done in either a **Site Collection** scoped feature or a **Site** scoped feature.

Locate the **public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)** method.

Add the following code, replacing **TenantResourcesMapping.Files** with the class name of your Files in the Resource Mappings class, and **MyFile** with the property representing your file.

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.Files>(q => q.MyFile)
			.WithSettingsForGenericFile()
			.SetFileName("MyFile.html")
			.DeploysTo(SharePointFileDeploymentTargets.StyleLibrary);
	}
	
Notice that in the **SetFileName** you can decide the filename of the uploaded file.

The **DeploysTo()** method can either be called,like above with a predefined library from the **SharePointFileDeploymentTargets** class, or with a relative path to a library

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.Files>(q => q.MyFile)
			.WithSettingsForGenericFile()
			.SetFileName("MyFile.html")
			.DeploysTo("/MyLibrary");
	}
	
Or even to a specific folder in a library

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.Files>(q => q.MyFile)
			.WithSettingsForGenericFile()
			.SetFileName("MyFile.html")
			.DeploysTo("/MyLibrary", "AFolder");
	}