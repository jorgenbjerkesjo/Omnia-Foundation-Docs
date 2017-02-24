Performance
============================

The following sections will guide you in using functionality in Omnia to improve the performance in your solution

ClientContext.LoadIfNeeded and ClientContext.ExecuteQueryIfNeeded
####

Often, a single SharePoint Client Context is passed between different services and you do not know if the data you need has already been loaded or if an **ExecuteQuery** is needed.

To make this process more effective and straight forward, Omnia has implemented two extension methods on **ClientContext**: **LoadIfNeeded** and **ExecuteQueryIfNeeded**.

By using the above methods, already loaded and fetched data will not trigger a new **ExecuteQuery** to the server.

Example:

.. code-block:: c#

	public string GetServerRelativeUrl(ClientContext ctx)
	{
		ctx.LoadIfNeeded(Ctx.Web, x => x.ServerRelativeUrl).ExecuteQueryIfNeeded();
		
		return ctx.Web.ServerRelativeUrl;
	}
	
The **ExecuteQueryIfNeeded** method also takes an optional parameter of type **Microsoft.SharePoint.Client.ClientContextExtensions.ExecuteOption**. This is an enum with two possible values:

- LoadAllIfOneIsNeeded
- LoadOnlyNeeded

This is useful to decide if only missing property should be loaded from the server, or if all loaded properties should be reloaded if an execute needs to be done.

If this parameter is not supplied, **LoadOnlyNeeded** is used.