Content Type
============================

Content Types are used in SharePoint to classify information. Omnia Foundation makes it easy to create content type definitions by using attributes that can be provisioned using an :doc:`Omnia feature </fundamentals/omnia-feature>` or referenced in a :doc:`List definition </sharepoint-provisioning/lists>`.

Content Types are hierarchical, meaning that new content types needs to inherit from an existing one. The base content type in SharePoint is **Item**.

Create a Content Type
###########

1. In your Visual Studio project, right click and choose **Add -> New item**
2. From the Omnia group, choose the **Content Type** template and give your content type class a name. Click **Add**

When doing this a new class is created in your project, inheriting from **Omnia.Foundation.Extensibility.ContentTypes.BuiltIn.Item**. This class represents the **Item** content type in SharePoint.

Inheritance
########

To make your content type inherit from another content type than **Item**, you can either choose to inherit from another class in the **Omnia.Foundation.Extensibility.ContentTypes.BuiltIn** or from any other content type class in your project.

Properties
######

The content type class is also decorated with a **ContentType** attribute. This attribute tells Omnia that your class should be treated as a content type and contains a number of required properties:

=================================  	=====================================================================================
Attribute                          	Description
=================================  	=====================================================================================
id									A unique GUID for the content type, adds as part of the ContentTypeId
name				              	The name of the content type. Could be a :doc:`Localization </fundamentals/localization>` string or a plain text string
Group					           	The group of the content type
Description			             	The description of the content type. Could be a :doc:`Localization </fundamentals/localization>` string or a plain text string
=================================  	=====================================================================================

There is also a large number of optional properties that can help you configure the content type the way you want.

Adding fields
######

To add fields to your content type you add properties to your class:

.. code-block:: c#

    [FieldRef(typeof(Preamble))]
    public string Preamble { get; set; }
	
In the **FieldRef** attribute you supply the class of an existing :doc:`Field </sharepoint-provisioning/fields>`

In the **FieldRef** attribute you can also specify a number of optional properties, to for example make the field required or hidden:

.. code-block:: c#
    
	[FieldRef(typeof(Preamble), Required = true)]
	public string Preamble { get; set; }
	
Provisioning
#####

As mentioned, a content type can be provisioned directly via an :doc:`Omnia feature </fundamentals/omnia-feature>`. This way the content type will be created as a Site Content Type.

It can also be provisioned via a List definition class. This way the content type will be a List Content Type.

**Provision Site Content Type**

In a site scoped feature, in the **OnSharePointArtifactMappings(SharePointArtifactMapper artifactMapper)** method, add the following code

.. code-block:: c#
	
	public override void OnSharePointArtifactMappings(SharePointArtifactMapper artifactMapper)
	{
		artifactMapper.MapToContentType<MyContentTypeClass>().
		ApplyChangeOn(Ctx.Web);
	}
	
You can also tell Omnia to add the content type to a list:

.. code-block:: c#
	
	public override void OnSharePointArtifactMappings(SharePointArtifactMapper artifactMapper)
	{
		artifactMapper.MapToContentType<MyContentTypeClass>().
		ApplyChangeOn(Ctx.Web).
		AddToList<Omnia.Foundation.Extensibility.Lists.Builtin.Pages>();
	}

**Provision List Content Type**

In a List class find the **public IEnumerable<ContentTypeBase> ContentTypes** property.

Decorate this property with a **ContentTypeRef** attribute

.. code-block:: c#

	[ContentTypeRef(typeof(MyContentTypeClass))]
	public IEnumerable<ContentTypeBase> ContentTypes
	{
		get { return GetContentTypes(); }
	}
	
In the **ContentTypeRef** attribute, supply the class of your content type, or the ContentTypeId of an existing content type.

The **ContentTypeRef** attribute also contains a number of optional parameters to allow you to for example make the content type the default one for the list.
