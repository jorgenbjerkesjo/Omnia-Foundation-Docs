Controls
============================

The Controls API contains methods that makes it easier to work with Omnia Controls.

You reach the Controls API through the following service

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Controls();
	

.. contents:: The API contains the following methods:
  :local:
  :depth: 1

GetControlSettings
#######

Use this method to get the stored settings for a given instance of an Omnia Control

.. code-block:: c#

	string OmniaApi.WorkWith(Ctx.Omnia()).Controls()
		.GetControlSettings(string scope, Guid controlId, string siteCollectionUrl, string siteUrl, int? pageItemId, Guid? featureResourceId);	

Pass in the scope of the control ("masterpage", "site", "page" or "webpart"), the Guid of the control, the url of the site collection and site.

To target a control on a certain page pass the a item ID of the page.

You can also pass in the feature resource Id of your control.