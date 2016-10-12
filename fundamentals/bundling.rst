Bundling
============================

Bundling is the process of concatinate multiple files into a single file. In web development, bundling can increase performance significantly, especially in high latency networks, by reducing the number of requests and round trips to the server. 

In Omnia feature you can specify which JavaScript and CSS resources to be bundled and which application the bundle is targeted for (more on this in bundle targets section). When that feature is activated, those resources will be add to the bundle corresponding to the feature's scope and the target application.

.. contents:: Sections:
  :local:
  :depth: 1

Creating bundles
--------------------------------------------------

To bundle resources, override the method OnTenantResourceMappings in your Omnia feature and use the method CreateBundleFor of the resourceMapper. 

.. code-block:: c#

    public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
    {
        resourceMapper
            .CreateBundleFor(Models.Features.BundleTargets.SharePoint)                            
            .Include<ResourcesMapping.Core>()
            .Include<ResourcesMapping.Services>(q => q.AjaxService)
            .Include<ResourcesMapping.Services>();
    }     

You can add specific file from the :doc:`resources mapping </fundamentals/resource-mappings.html>`

.. code-block:: c#

    .Include<ResourcesMapping.Services>(q => q.AjaxService)

Or you can add a whole folder and the resourceMapper will recursively include every files in that folder. The resource mapper will be smart enough to not include a file again if it is already in the bundle

.. code-block:: c#

    .Include<ResourcesMapping.Services>(q => q.AjaxService)
    .Include<ResourcesMapping.Services>()

The resources will be bundled in the same order as they were included here. In this example, the resources will be bundled in the following order:

- All the files (recursively including child folders) in Core folder
- The ajaxService file in Services folder 
- All the files in Services folder (recursively including child folders) except for ajaxService

Bundle scopes
--------------------------------------------------

There are 3 bundle scopes: Tenant, Site Collection and Site. These scopes map with Omnia feature scopes, meaning an tenant-scope feature can only add resources to the tenant-scope bundle, etc.

On each Omnia page load (in SharePoint or in admin web), these 6 bundle files that will be loaded:

+------------------+---------------------------------------------------------------+----------------------------------------------------------+
| Scope            | Bundle                                                        | Content                                                  |
+==================+===============================================================+==========================================================+
| Site             | - site.js                                                     | | All resources bundled by site-scope features           |
|                  | - site.css                                                    | | activated on the current site                          |
|                  |                                                               |                                                          |
|                  |                                                               |                                                          |
|                  |                                                               |                                                          |
+------------------+---------------------------------------------------------------+----------------------------------------------------------+
| SiteCollection   | - sitecollection.js                                           | | All resources bundled by sitecollection-scope features | 
|                  | - sitecollection.css                                          | | activated on the current site collection               |
|                  |                                                               |                                                          |
+------------------+---------------------------------------------------------------+----------------------------------------------------------+
| Tenant           | - tenant.js                                                   | | All resources bundled by activated tenant-scope        |
|                  | - tenant.css                                                  | | features                                               |
|                  |                                                               |                                                          |
+------------------+---------------------------------------------------------------+----------------------------------------------------------+


Bundle targets
--------------------------------------------------

Currently there are only two bundle targets, SharePoint and OmniaAdmin. As the names imply, resources bundles targeting SharePoint will only be loaded in SharePoint and the resources bundle targeting OmniaAdmin will only be loaded in Omnia admin web.

A general guideline on bundle targets would be:

- JavaScript services and directives usually can target both SharePoint and Omnia Admin.
- Omnia controls should only target SharePoint.
- Admin settings should only target OmniaAdmin.

.. code-block:: c#

    public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
    {
        resourceMapper
            .AddOrUpdateTenantResourcesFrom<TenantResources>();

        resourceMapper
            .CreateBundleFor(Models.Features.BundleTargets.SharePoint)                
            .Include<ResourcesMapping.Enums>()
            .Include<ResourcesMapping.Core>()
            .Include<ResourcesMapping.Services>()
            .Include<ResourcesMapping.Directives>()
            .Include<ResourcesMapping.Styles>();

        resourceMapper
            .CreateBundleFor(Models.Features.BundleTargets.OmniaAdmin)                
            .Include<ResourcesMapping.Enums>()
            .Include<ResourcesMapping.Core>()
            .Include<ResourcesMapping.Services>()
            .Include<ResourcesMapping.Directives>()
            .Include<ResourcesMapping.Styles>()
            .Include<ResourcesMapping.AdminSettings.Controllers>()
            .Include<ResourcesMapping.AdminSettings>();        
    }     

Bundle sequence number
--------------------------------------------------

While you can specify the order of resources in your feature just by order they were included, sometimes you will also need to ensure the resources of one feature is loaded before the resources of other features. For that purpose you can set the sequence number for your feature bundle: 

.. code-block:: c#

    public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
    {
        resourceMapper
            .AddOrUpdateTenantResourcesFrom<TenantResources>();

        resourceMapper
            .CreateBundleFor(Models.Features.BundleTargets.SharePoint)                
            .Include<ResourcesMapping.Enums>()
            .Include<ResourcesMapping.Core>()
            .Include<ResourcesMapping.Services>()
            .Include<ResourcesMapping.Directives>()
            .Include<ResourcesMapping.Styles>();

        resourceMapper
            .CreateBundleFor(Models.Features.BundleTargets.OmniaAdmin)                
            .Include<ResourcesMapping.Enums>()
            .Include<ResourcesMapping.Core>()
            .Include<ResourcesMapping.Services>()
            .Include<ResourcesMapping.Directives>()
            .Include<ResourcesMapping.Styles>()
            .Include<ResourcesMapping.AdminSettings.Controllers>()
            .Include<ResourcesMapping.AdminSettings>();
        
        resourceMapper
            .SetBundlesSequence(90000, Models.Features.BundleTargets.SharePoint)
            .SetBundlesSequence(80000, Models.Features.BundleTargets.OmniaAdmin);
    }     

The bundle with lower sequence number will be included first in the bundle. The default sequence number is 100000. You should not set the sequence number to lower than 100 because the sequence numbers from 0 to 100 are reserved for core features of Omnia Foundation.

Also, from the example you can see that the sequence number can be different for bundling targets.

Bundle minification
--------------------------------------------------

In non-development environments, all JavaScript bundles will be `minified <https://en.wikipedia.org/wiki/Minification_(programming)>`_ to reduce the size of the bundles and further improve performance. However, this sometimes can cause issues if the code was not written in a way that is compatible with minification. If you have errors happened only in non-development environments and you suspect it could be from the minification, use the querystring parameter "**debug=true**" to un-minify your code.

One common issue with minification is Angular dependencies injection. For example, this code will not work when minified

.. code-block:: javascript

    var app = angular.module('bigApp', []);

    app.controller('mainController', function($scope) {
        $scope.message = 'OH NO!';  
    });

But this will code will

.. code-block:: javascript

    var app = angular.module('bigApp', []);

    app.controller('mainController', ['$scope', function($scope) {
        $scope.message = 'HOORAY!'; 
    }]);       


To understand why the second code block works with minification while the first does not, read `this article <https://scotch.io/tutorials/declaring-angularjs-modules-for-minification>`_.


