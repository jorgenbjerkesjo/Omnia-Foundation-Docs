Generic File
============================

There could be a need in your project to provision other kinds of files than the ones outlined in the previous sections. 
This can be done in either a **Site Collection** scoped feature or a **Site** scoped feature.

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