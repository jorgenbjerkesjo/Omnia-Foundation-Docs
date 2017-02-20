List
============================

A List is one of the core SharePoint artifacts and Omnia Foundation makes it easy to create list definitions by using attributes that can be provisioned using :doc:`Omnia features </fundamentals/omnia-feature>`.

Create a List
#####

1. In your Visual Studio project, right click and choose **Add -> New item**
2. From the Omnia group, choose the **List** template and give your list class a name. Click **Add**

When doing this a new class is created in your project, inheriting from **Omnia.Foundation.Extensibility.Lists.ListBase** and implementing the **Omnia.Foundation.Extensibility.Lists.IListBase** interface.

The class is also decoreated with a **List** attribute with a number of required properties

Properties
#####

The List attribute has the following required properties

=================================  	=====================================================================================
Attribute                          	Description
=================================  	=====================================================================================
url									The site relative path of the list
title				              	The title of the list. Could be a :doc:`Localization </fundamentals/localization>` string or a plain text string
listTemplate					    The list template type to inherite from, defaults to **ListTemplateType.GenericList**
=================================  	=====================================================================================

There is also a large number of optional properties that can help you configure the list the way you want.

Adding Fields
######

You can add :doc:`Fields </sharepoint-provisioning/fields>` to your list by following the steps below.

In the List class find the **public IEnumerable<FieldBase> Fields** property.

Decorate this property with a **FieldRef** attribute

.. code-block:: c#

	[FieldRef(typeof(LinkTitle))]
	public IEnumerable<FieldBase> Fields
	{
		get { return GetFields(); }
	}

	
In the **FieldRef** attribute, supply the class of your field or a class from **Omnia.Foundation.Extensibility.Fields.BuiltIn**.

The **FieldRef** attribute also contains a number of optional parameters to allow you to for example make the field required in the list.

Adding Content Types
######

You can add :doc:`Content Types </sharepoint-provisioning/content-types>` to your list by following the steps below.

In the List class find the **public IEnumerable<ContentTypeBase> ContentTypes** property.

Decorate this property with a **ContentTypeRef** attribute

.. code-block:: c#

	[ContentTypeRef(typeof(MyContentTypeClass))]
	public IEnumerable<ContentTypeBase> ContentTypes
	{
		get { return GetContentTypes(); }
	}
	
In the **ContentTypeRef** attribute, supply the class of your content type, or the ContentTypeId of an existing content type.

The **ContentTypeRef** attribute also contains a number of optional parameters to allow you to for example make the content type the default one for the list.

Setup the default view
#####

Omnia also provides logic to configure the default view of the list.

In the Lists class find the **public IEnumerable<FieldBase> DefaultView** property. Decorate it with **FieldRef** attributes where you supply the type for the field and the index you want the field to have in the view.

.. code-block:: c#

	[FieldRef(typeof(DocIcon), 1)]
	[FieldRef(typeof(LinkTitle), 2)]
	[FieldRef(typeof(Modified), 3)]
	[FieldRef(typeof(Author), 4)]
	public IEnumerable<FieldBase> DefaultView
	{
		get { return GetDefaultViewFields(); }
	}

Provisioning
#####

A list is provisioned directly via a **site scoped** :doc:`Omnia feature </fundamentals/omnia-feature>`

In the **OnSharePointArtifactMappings(SharePointArtifactMapper artifactMapper)** method, add the following code

.. code-block:: c#
	
	public override void OnSharePointArtifactMappings(SharePointArtifactMapper artifactMapper)
	{
		artifactMapper
			.MapToList<MyListClass>()
			.DeployTo(Ctx.Web);
	}

