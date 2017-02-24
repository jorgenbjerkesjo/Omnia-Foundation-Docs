Page Layout
============================

Page layouts in SharePoint are stored in the Master Page gallery on the root web of a site collection. Therefore it is recommended to provision page layout files in a Site Collection scoped feature.

In a Site Collection scope feature, locate the **public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)** method.

Add the following code, replacing **TenantResourcesMapping.PageLayouts** with the class name of your Page layout in the Resource Mappings class, and **StartPage** with the property representing your page layout.

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.PageLayouts>(q => q.StartPage)
			.WithSettingsForPageLayout()
			.DeploysTo(SharePointFileDeploymentTargets.MasterPageGallery);
	}

Note that this is a fluent API where you after the **.WithSettingsForPageLayout()** can modify a number of additional things about how the file will be provisioned, for example the file name

.. code-block:: c#

	public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
	{
		resourceMapper.MapTenantResource<TenantResourcesMapping.PageLayouts>(q => q.StartPage)
			.WithSettingsForPageLayout()
			.SetFileName("StartPageLayout.aspx")
			.DeploysTo(SharePointFileDeploymentTargets.MasterPageGallery);
	}
