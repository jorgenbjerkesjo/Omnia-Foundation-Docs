Provisioning Pipeline
============================

Omnia provides a way of doing modifications to resources provisioned as part of core Omnia features. 

A Provisioning Pipeline makes it possible to modify the content of a resource file at the time it is provisioned to a SharePoint site. This enables you to adapt those resources to specific needs.

.. note:: At this time, Provisioning Pipeline only applies to the Omnia master page.

Add a Provisioning Pipeline
#######

To add a new Provisioning Pipline to your project, add a new class inheriting from **SharePointFileProvisioningPipeline** (found in Omnia.Foundation.Extensibility.TenantResources).

Decorate it with a **SharePointFileProvisioningPipelineDefinition** attribute and a unique GUID:

.. code-block:: c#

  using Omnia.Foundation.Extensibility.TenantResources;
  
  [SharePointFileProvisioningPipelineDefinition(id: "2F5450A8-8C6E-4C8F-A907-4F285691BEEB")]
  public class MyMasterPageProvisioningPipeline : SharePointFileProvisioningPipeline
  { 
    
  }
  
  
Methods
######

The **SharePointFileProvisioningPipeline** class has two overridable methods:

.. code-block:: c#

   public override string BeforeContentReplacements(string content, TenantResource tenantResource)

.. code-block:: c#
   
   public override string AfterContentReplacements(string content, TenantResource tenantResource)


The **content** parameter of the methods contains the full file content in a string format. This allows you to do string operations like **Contains** and **Replace**. Omnia also provides a number of extensions to the String object in C#, like **InsertBefore** and **InsertAfter**.

The **tenantResource** parameter contains informmation about the resource that is currently being processed. You can do comparisons with this parameter to make sure your modifications only affects a specific Omnia resource:

.. code-block:: c#

  public override string BeforeContentReplacements(string content, TenantResource tenantResource)
  {
      var portalMasterPageId = new Guid(BuiltInResources.MasterPages.Omnia);

      if (tenantResource.Id == portalMasterPageId)
      {
          // Do code modifications to content here
      }
  }

The main difference between the **BeforeContentReplacements** and **AfterContentReplacements** is that in the **BeforeContentReplacements** the internal tokens Omnia uses to insert functionality to the files are still present in the content string. 

The content in the **AfterContentReplacements** resembles exactly the content of the file that is provisioned to SharePoint.

Example of a BeforeContentReplacements method
#####

.. code-block:: c#

  public override string BeforeContentReplacements(string content, TenantResource tenantResource)
  {
      var portalMasterPageId = new Guid(BuiltInResources.MasterPages.Omnia);

      if (tenantResource.Id == portalMasterPageId)
      {
          try
          {
               content = content.InsertBefore(ProvisioningPipelineTokens.BodyContainerTop, "<div class=\"myClass\">Hello World</div>"); 
          }
          catch(Exception ex)
          {
              return content.InsertBefore(ProvisioningPipelineTokens.GlobalNavLeft, ex.Message);
          }
      }
	
      return content;
  }

.. note:: The **ProvisioningPipelineTokens** class contains the internal tokens still present in the resource files in the **BeforeContentReplacements** method 
  
Example of an AfterContentReplacements method
#####

.. code-block:: c#

  public override string AfterContentReplacements(string content, TenantResource tenantResource)
  {
      var portalMasterPageId = new Guid(BuiltInResources.MasterPages.Omnia);

      if (tenantResource.Id == portalMasterPageId)
      {
          try
          {
              content = content.InsertBefore("<div class=\"myClass\">", "<div>This is added in front of the Hello World tag added in the BeforeContentReplacements method</div>");
          }
          catch (Exception ex)
          {
              content.InsertBefore("</head>", "<div>" + ex.Message + "</div>");
          }
      }

      return content;
  }


