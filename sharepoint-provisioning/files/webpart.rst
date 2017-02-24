Webpart Definition
============================

Deploying a webpart definition is very similar to provisioning a page layout. Web part definitions are also stored in the root web of a site collection, but in the Webpart gallery library. Therefore it is recommended to provision page layout files in a Site Collection scoped feature.

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