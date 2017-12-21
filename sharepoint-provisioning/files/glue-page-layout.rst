Glue Layout
============================

Glue layouts in Omnia are used as layouts for Quick Pages. 

They are stored in the Master Page gallery on the root web of a site collection. Therefore it is recommended to provision page layout files in a Site Collection scoped feature.

In a Site Collection scope feature, locate the **public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)** method.

Add the following code, replacing **TenantResourcesMapping.GlueLayouts** with the class name of your Glue layout in the Resource Mappings class, and **StartPage** with the property representing the specific glue layout.

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.GlueLayouts>(q => q.StartPage)
			.WithSettingsForGlueLayout()
			.DeploysTo(SharePointFileDeploymentTargets.MasterPageGallery);
	}

Note that this is a fluent API where you after the **.WithSettingsForGlueLayout()** can modify a number of additional things about how the file will be provisioned, for example the file name

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.GlueLayouts>(q => q.StartPage)
			.WithSettingsForGlueLayout()
			.SetFileName("StartPageLayout.aspx")
			.DeploysTo(SharePointFileDeploymentTargets.MasterPageGallery);
	}
	
or make it the default layout for new quick pages

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.GlueLayouts>(q => q.StartPage)
			.WithSettingsForGlueLayout()
			.SetAsDefault()
			.DeploysTo(SharePointFileDeploymentTargets.MasterPageGallery);
	}
	
You can also add tokens in your layouts that can be replaced when the file is provisioned to SharePoint

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.GlueLayouts>(q => q.StartPage)
			.WithSettingsForGlueLayout()
			.TokenReplace("[WeWantTheWebsTitleHere]", Ctx.Web.Title)
			.DeploysTo(SharePointFileDeploymentTargets.MasterPageGallery);
	}
