Security
============================

.. note:: This is a draft version of the documentation, it is not yet complete. Feel free to contribute to it via GitHub.

The Security API allows you to work with permissions, both Omnia permissions and SharePoint permissions.

You can for example check if the current user has a certain permission on a web, or a certain role in Omnia.

You reach the Security API through the following service

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx);

.. contents:: The API contains the following methods:
  :local:
  :depth: 1

AddOrUpdateOmniaPermissionRoles
#######

DeleteOmniaPermissionRolesForExtensionPackage
#######

DoesUserHavePermissionOnWeb
#######

Use this method to check if the user has the specified permission on the current web.

.. code-block:: c#

	bool OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx)
		.DoesUserHavePermissionOnWeb(PermissionKind permissionKind);
		
Returns **true** if the user has the supplied **permissionKind**, otherwise **false**.

GetAllOmniaPermissionRoles
#######

Use this method to get all available Omnia Permission roles for a given URL.

.. code-block:: c#

	IEnumerable<PermissionRole> OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx)
		.GetAllOmniaPermissionRoles(string targetUrl); 
		
Returns an array of **PermissionRole** for the given **targetUrl**.

GetCurrentUserADGroups
#######

If you need to get all AD groups the current user is a member of, use the following end-point.

.. note:: Only AD groups used somewhere in Omnia Foundation will be returned, e.x. groups used for security in Omnia Administration or as part of targeting definitions.

.. code-block:: c#

	IEnumerable<string> OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx)
		.GetCurrentUserADGroups(string[] groups); 

If you supply and empty **groups** parameter, you will get all AD groups, used somewhere in Omnia, that the user is a member of.

.. code-block:: c#

	var allGroups = OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx)
		.GetCurrentUserADGroups(new string[0]()); 

To only check specific AD groups, pass their names in the **groups** parameter

.. code-block:: c#

	var someGroups = OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx)
		.GetCurrentUserADGroups(new string{ "ADGroup1", "ADGroup2" }); 

GetPermissionRoles
#######

To get Permissions Roles for given Permission Role Definitions you can use the following method, passing in a **List<PermissionRoleDefinition>** containing the role definitions of interest.

.. code-block:: c#

	IEnumerable<PermissionRole> OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx)
		GetPermissionRoles(List<PermissionRoleDefinition> requestedRoles); 
		
This will return an array containing all permissions roles existing for the supplied definitions.

IsUserAuthorized
#######

Use this method to check if the user is authorized, e.g. is a member of the required Omnia **PermissionRoleDefinition**

.. code-block:: c#

	bool OmniaApi.WorkWith(Ctx.Omnia()).Security(Ctx)
		.IsUserAuthorized(PermissionRoleDefinition requiredRole);
		
Returns **true** if the user is a member of the required Omnia role, else **false**.

SearchOmniaPermissionRoles
#######