Resource Mappings
============================

In an Omnia extension, every resource files need to be mapped with a unique ID. The resource mappings are defined with an attribute-based pattern like other entities in Omnia like feature, jobs and SharePoint artifacts.

.. image:: /images/extensionpackage-resourcemapping.png

The resource mappings aslo help create an logical hierachy or grouping of resources, which can be used to distribute the resources to different Omnia features. If your extension is small (less than 50 resource files) you should put all resource mappings into one file, otherwise you should split it into multiple files by feature area.  

In the resource mappings you can also specify different properties and metadata.

Folders mapping
##################################################

.. code-block:: c#

  [TenantResourceFolderMapping(id: "EE2FFD07-D5B6-4964-BD46-09472229F49C", name: "Scripts")]
  public class Scripts
  { 
    // Other folders or files mapping code in here
  }

        
Generic files mapping
##################################################

.. code-block:: c#

  [TenantResourceFileMapping(id: "DC37115E-9ED7-4017-BCA1-449C67D8EBC0", 
    sourceRelativePath: "/TenantResources/Scripts/Core/sample.core.js")]
  public string SampleCoreJs { get; set; }

Site templates mapping
##################################################

.. code-block:: c#
  
  [TenantResourceFileMapping(id: "AA5A09DA-CA8D-4289-9EF2-D05AE99E4AB2", 
    sourceRelativePath: "/TenantResources/SiteTemplates/sampletemplate.json", 
    Category = BuiltInCategories.SiteProvisioning.SiteTemplate)]
  public string SampleTemplate { get; set; }

Localization files mapping
##################################################

.. code-block:: c#

  [TenantResourceFileMapping(id: "766F18D7-1E6B-486D-912B-C12D6683B28A", 
    sourceRelativePath: "/TenantResources/Localization/sample.loc.json")]
  public string SampleLocalization { get; set; }

  [TenantResourceFileMapping(id: "0CD92A43-9065-49DC-83EB-054123445A9E", 
    sourceRelativePath: "/TenantResources/Localization/sample.loc.sv-se.json")]
  public string SampleLocalizationSvSe { get; set; }

SharePoint masterpages mapping
##################################################

.. code-block:: C#

  [TenantResourceFileMapping(id: "69B74E73-CDA2-4BB9-A917-C16F2FACB5D1", 
    sourceRelativePath: "/TenantResources/MasterPages/sample.master")]
  [ContentTypeId(typeof(Omnia.Foundation.Extensibility.ContentTypes.BuiltIn.MasterPage))]
  [SharePointFileProperty("MasterPageDescription", "")]
  [SharePointFileProperty("UIVersion", "15")]
  public string PortalMaster { get; set; }

SharePoint webparts mapping
##################################################

.. code-block:: c#

 [TenantResourceFileMapping(id: "5486D161-E899-4AC5-BBCE-2F4B093B788C", 
   sourceRelativePath: "/TenantResources/WebParts/sample.webpart")]
 [SharePointFileProperty("Group", Omnia.Foundation.Core.Constants.WebPartGroups.Omnia)]
 public string SampleWebPart { get; set; }

SharePoint pagelayouts mapping
##################################################

.. code-block:: c#

 [TenantResourceFileMapping(id: "94D169CF-B8F0-4A55-9767-5F410DBAC9F5", 
   sourceRelativePath: "/TenantResources/PageLayouts/SamplePageLayout.aspx")]
 [ContentTypeId(typeof(Omnia.Foundation.Extensibility.ContentTypes.BuiltIn.PageLayout))]
 [PublishingAssociatedContentType(typeof(ArticlePage))]
 [SharePointFileProperty("Title", 
   "$Localize:MyOmniaExtension.Sample.PageLayouts.SamplePageLayout.Title;")]
 public string SamplePageLayout { get; set; }


Working with resource mappings
--------------------------------------------------

When developing new extension, after you have all the resources ready, you can create a new resource mapping class using Omnia toolings

.. image:: /images/toolings-item-templates-resourcemapping.png

.. image:: /images/toolings-item-templates-resourcemapping2.png

After the resource mapping class has been created, you can manually write code to map your resource files following that sample pattern here, or you can again use Omnia toolings to generate the mapping code for you. Right-click on the folder contains your new resources and select **Create Resource Mappings**

.. image:: /images/toolings-create-resourcemappings.png

The generated mapping code has been copy to your clipboard, now go to the new **ScriptMappings** class you have created ealier and replace the sample code with the correct mapping

.. image:: /images/toolings-create-resourcemappings2.png

.. image:: /images/toolings-create-resourcemappings3.png



