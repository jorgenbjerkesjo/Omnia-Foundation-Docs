Omnia Feature
============================

Omnia features are modules containing customizations for Omnia. They are very similar to SharePoint features, except that they can provisioning resources to both SharePoint and to Omnia database. An Omnia feature can also run your custom code when they are activated, upgraded or removed.

.. contents:: Sections:
  :local:
  :depth: 1

Feature scopes
--------------------------------------------------

A feature will have one of the three scopes **Site**, **SiteCollection** and **Tenant**, each scope can deploy different types of artifacts.

+------------------+---------------------------------------------------------------+---------------------------+
| Scope            | Artifacts                                                     | SharePoint ClientContext  |
+==================+===============================================================+===========================+
| Site             | - Page                                                        | | App-only context of the |
|                  | - List instance                                               | | target site             |
|                  | - `Site.js bundle </fundamentals/bundling.html>`_             |                           |
|                  | - `Site.css bundle </fundamentals/bundling.html>`_            |                           |
|                  |                                                               |                           |
+------------------+---------------------------------------------------------------+---------------------------+
| SiteCollection   | - Page                                                        | | App-only context of the | 
|                  | - List instance                                               | | root site of the target |
|                  | - Content Type                                                | | site collection         |
|                  | - Field                                                       |                           |
|                  | - Masterpage and pagelayout                                   |                           |
|                  | - Webpart                                                     |                           |
|                  | - `SiteCollection.js bundle </fundamentals/bundling.html>`_   |                           |
|                  | - `SiteCollection.css bundle </fundamentals/bundling.html>`_  |                           |
|                  |                                                               |                           |
+------------------+---------------------------------------------------------------+---------------------------+
| Tenant           | - Tenant resource                                             | | Not available           |
|                  | - `Tenant.js bundle </fundamentals/bundling.html>`_           |                           |
|                  | - `Tenant.css bundle </fundamentals/bundling.html>`_          |                           |
+------------------+---------------------------------------------------------------+---------------------------+

Built-in methods and properties
--------------------------------------------------


+-------------------+---------------+---------------------------------------------------------------+
| Properties        | Type          | Description                                                   |
+===================+===============+===============================================================+
| Ctx               | ClientContext | | App-only client context for the targeting site.             |
|                   |               | | This property is not available in tenant scope features.    |
|                   |               |                                                               |
+-------------------+---------------+---------------------------------------------------------------+


+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+
| Methods                                                            | Type          | Description                                                   |
+====================================================================+===============+===============================================================+
| CreateContextFor(string spUrl)                                     | ClientContext | | Create a new app-only context for another site              |
|                                                                    |               |                                                               |
+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+
| Log(string log)                                                    | void          | | Add a new entry to this feature's logs                      |
|                                                                    |               |                                                               |
+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+
| WorkWith()                                                         | ApiFactory    | | Return the ApiFactory that can call Omnia API               |
|                                                                    |               | | Example: WorkWith().Logging().AddLog(log)                   |
+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+
| Localize(string localizeKey)                                       | string        | | Return the localized string                                 |
|                                                                    |               |                                                               |
+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+


Create new feature
--------------------------------------------------

As usual, you can create new Omnia feature using the template from Omnia tooling 

.. image:: /images/toolings-item-templates-feature.png

At the top of the feature is the **FeatureDefinition** attribute which contains the feature's metadata

.. code-block:: c#

    [FeatureDefinition(
       id: "85544C6C-9EB9-4F99-9410-95F1EA3D07B5",
       name: "MyOmniaExtension Sample Feature Core",
       version: "0.1.0",
       scope: FeatureScopes.Tenant
    )]
    public class SampleFeatureCore : Omnia.Foundation.Extensibility.Features.OmniaFeature    

Feature activation, deactivation and upgrade
--------------------------------------------------

In a feature you can override the activation, decativation or upgrade events to run custom code. In these events, you can:

- Write CSOM code to work with data in SharePoint (except for Tenant scope features where the ClientContext is not available)
- Call the API of Omnia Foundation like `logging </fundamentals/logging.html>`_ and `configurations </fundamentals/configuration.html>`_ using the built-in method **WorkWith**
- Trigger queue jobs using Omnia Queues API


**Example**: Provisioning new start page for the target site when activating the feature

.. code-block:: c#

    /// <summary>
    /// Activates the feature
    /// </summary>
    public override void Activate()
    {
        try
        {            
            string pageTitle = this.Localize("$Localize:MyOmniaExtension.StartPageTitle;");            
            var publishingClient = this.WorkWith().Publishing(this.Ctx);
            publishingClient.CreateStartPage(new Page
            {
                Name = "StartPage.aspx",
                PageLayout = MyOmniaExtension.PageLayouts.StartPage,
                Title = pageTitle
            });            
        }
        catch (Exception ex)
        {
            this.Log("Activate feature", ex.Message, FeatureInstanceLogTypes.Error);
            throw;
        }
    }

    /// <summary>
    /// Deactivates the feature.
    /// </summary>
    /// <param name="fromVersion">From version.</param>
    public override void Deactivate(string fromVersion)
    {
        // Your code to handle feature deactivation here
    }

    /// <summary>
    /// Upgrades the feature
    /// </summary>
    /// <param name="fromVersion">From version.</param>
    public override void Upgrade(string fromVersion)
    {
        // Your code to handle feature upgrade here
    }


Provisioning tenant resources
--------------------------------------------------

By overriding the method **OnTenantResourceMappings**, a feature can provision tenant resources to Omnia database.

For code resources like JavaScript and CSS, you will also need to add them to a bundle for them to be loaded and executed on SharePoint or Omnia admin application. Read more on `bundling in Omnia </fundamentals/bundling.html>`_.

.. note:: Only tenant-scope features can provision tenant resources, though any features can add resources to bundles. 

**Example**

.. code-block:: c#

    /// <summary>
    /// Called when [OmniaFeature resource mappings is being performed].
    /// </summary>
    /// <param name="resourceMapper">The resource mapper.</param>
    public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
    {
        // Provisioning tenant resources to Omnia database
        // NOTE: Only tenant-scope features can provision tenant resources
        resourceMapper
            .AddOrUpdateTenantResourcesFrom<ResourcesMapping>();

        // Adding resources to the scope's bundles, in this case tenant.js and tenant.css        
        resourceMapper
            .CreateBundleFor(BundleTargets.SharePoint)
            .Include<ResourcesMapping.Scripts.Core>()
            .Include<ResourcesMapping.Scripts.Services>()
            .Include<ResourcesMapping.Scripts.Directives>()
            .Include<ResourcesMapping.Scripts.Controls>()
            .Include<ResourcesMapping.Styles>();

        resourceMapper
            .CreateBundleFor(BundleTargets.OmniaAdmin)
            .Include<ResourcesMapping.Scripts.Core>()
            .Include<ResourcesMapping.Scripts.Services>()
            .Include<ResourcesMapping.Scripts.Directives>()
            .Include<ResourcesMapping.Scripts.AdminSettings>(q => q.SampleAdminSettingsFormJs)
            .Include<ResourcesMapping.Scripts.AdminSettings>(q => q.SampleAdminSettingsJs)
            .Include<ResourcesMapping.Scripts.AdminSettings>();
    }

Provisioning SharePoint artifacts
--------------------------------------------------

In the method **OnTenantResourceMappings** you can also provision files like masterpage and pagelayout to SharePoint. To provision other SharePoint artifacts like fields, content types and list instances you need to override the method **OnSharePointArtifactMappings**.

.. note:: Only site-scope and sitecollection-scope features can provision SharePoint artifacts

**Example**

.. code-block:: c#

    /// <summary>
    /// Called when [OmniaFeature resource mappings is being performed].
    /// </summary>
    /// <param name="resourceMapper">The resource mapper.</param>
    public override void OnTenantResourceMappings(TenantResourcesMapper resourceMapper)
    {
        resourceMapper
            .MapTenantResource<ResourcesMapping.PageLayouts>(q => q.SamplePageLayout)
            .WithSettingsForPageLayout()
            .DeploysTo(SharePointFileDeploymentTargets.MasterPageGallery);
    }

    /// <summary>
    /// Called when [OmniaFeature sharepoint artifacts mappings is being performed].
    /// </summary>
    /// <param name="artifactMapper">The artifacts mapper.</param>
    public override void OnSharePointArtifactMappings(SharePointArtifactMapper artifactMapper)
    {
        artifactMapper.MapToField<SampleField>()
            .DeployTo(Ctx.Site.RootWeb);

        artifactMapper
            .MapToContentType<SampleContentType>()
            .DeployTo(Ctx.Site.RootWeb)
            .UpdateChildren();

        artifactMapper
            .MapToList<SampleList>()
            .DeployTo(Ctx.Site.RootWeb);
    }    