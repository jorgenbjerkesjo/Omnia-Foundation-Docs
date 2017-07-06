Features
============================

The Features API allows you to Activate, Deactivate and in other ways work with :doc:`Omnia Features </fundamentals/omnia-feature>`.

.. note:: This API works with Omnia Features, not SharePoint Features.

You reach the Features API through the following service

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Features();
	

.. contents:: The API contains the following methods:
  :local:
  :depth: 1

ActivateFeature
#######

To activate an Omnia Feature, use the following method

.. code-block:: c#

	FeatureInstance OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.ActivateFeature(Guid id, string spUrl, bool force);
	
Supply the unique Guid of the feature, the URL of the tenant authority / site collection / web where the feature should be activated. Set the **force** parameter to **true** to ignore errors on activation.

DeactivateFeature
#######

To deactivate an Omnia Feature, use the following method

.. code-block:: c#

	FeatureInstance OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.DeactivateFeature(Guid id, string spUrl);
		
Supply the unique Guid of the feature and the URL of the tenant authority / site collection / web where the feature should be deactivated.		

GetFeature
#######

To get more information about a specific feature, use the following method

.. code-block:: c#

	FeatureModel OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.GetFeature(Guid id);

Supply the unique Guid of the feature. In return you get a **Omnia.Foundation.Models.Features.FeatureInstance** object containing feature details like Name, Description and Scope

GetFeatures
#######

To get more information about a all existing features, use the following method

.. code-block:: c#

	IEnumerable<FeatureModel> OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.GetFeatures();
		

GetFeatureActivationStatus
#######

To check if a given feature is activated or not on a site / site collection / tenant, use the following method

.. code-block:: c#

	FeatureInstanceStatus OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.GetFeatureActivationStatus(Guid id, string spUrl);

This will return a value from the **Omnia.Foundation.Models.Features.FeatureInstanceStatus** enumeration.

This can have any of the following values:

- NotActivated
- Activating
- Activated
- Upgrading
- Deactivating
- Error


GetFeatureInstanceLogs
#######

To get the log messages written for a specific feature, use the following method

.. code-block:: c#

	IEnumerable<FeatureInstanceLog> OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.GetFeatureInstanceLogs(Guid id, DateTimeOffset? startingBefore = default(DateTimeOffset?), int take = -1);

GetFeatureInstances
#######

To get all instances of a feature (e.g. all places where a feature is activated), use the following method

.. code-block:: c#

	IEnumerable<FeatureInstance> OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.GetFeatureInstances(Guid id);
		
This will return a collection of **Omnia.Foundation.Models.Features.FeatureInstance**, containing for example Status and Target of the feature

UpgradeFeature
#######


To upgrade a feature on one target (Tenant / Site collection / Site), use the following method

.. code-block:: c#

	FeatureInstance OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.UpgradeFeature(Guid id, string spUrl);

To upgrade features in multiple places at once, you can use the following method

.. code-block:: c#

	Dictionary<string, ApiOperationResult> OmniaApi.WorkWith(Ctx.Omnia()).Features()
		.UpgradeFeature(Guid id, List<string> spUrls);

This will update the feature in all of the instances specified by the list of URLs supplied in the **spUrls** parameter.