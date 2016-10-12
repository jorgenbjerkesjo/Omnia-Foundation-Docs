Permissions
============================

Omnia permission roles
--------------------------------------------------

Omnia permission roles are security groups for managing access rights to Omnia resources. Permission roles can also be used to limit the access to the API of Omnia extensions. An Omnia permission role can be assign to a user or a security group in Office 365 and can be apply to site, site collection or tenant scope.

.. contents:: Sections:
  :local:
  :depth: 1

Create form for managing permission roles in Omnia extension
--------------------------------------------------

Omnia toolings provides a template for managing permission roles in admin app for each feature

.. image:: /images/toolings-item-templates-permissionpage.png

The permission settings page is the same as any other admin app settings page in an Omnia extensions. The first notable part is the **navigationNode**. In the navigationNode, along with other metadata, you need to specify the **authorizedRoles**: the permission roles that can access this page and manage other permission roles. Usually, the authorizedRoles for permission settings pages are set to the OmniaAdmin.

.. code-block:: TypeScript

    static navigationNode: NavigationNode = {
        title: "", 
        state: "AdminSettingsPermissionsPagePermissions",
        url: "permission",
        controller: AdminSettingsPermissionsPagePermissionsController.ngName,
        viewId: "",
        iconClass: "fa-key", 
        authorizedRoles: [
            {
                name: Omnia.Foundation.Security.PermissionRoles.OmniaAdmin,
                scope: Omnia.Foundation.Security.PermissionScopes.Tenant
            }
        ]
    };

You will need to register this navigationNode to the navigation system of admin app. For more information about this read the article `admin settings pages <#>`_.

.. code-block:: TypeScript

    var node: NavigationNode = SampleSettings.SampleSettingsController.navigationNode;
    if (node.children == null)
        node.children = [];

    node.children.push(SampleSettings.AdminSettingsPermissionsPagePermissionsController.navigationNode);
    $admin.Navigation.addNode(NavigationScope.tenant, node);

The last and most important part is the roleDefinitionList property on $scope where you define all permission roles that can be managed on this page.

.. code-block:: TypeScript

     private init = () => {
        this.$scope.roleDefinitionList = [
            {
                name: "AdminSettingsPermissionsPage.Admin",
                scope: PermissionScopes.Tenant,
                label: "AdminSettingsPermissionsPage.Permissions.Admin.Label",
                description: "AdminSettingsPermissionsPage.Permissions.Admin.Description"
            }
        ];
    }


Deploy the extension and the permissions settings should be available

.. image:: /images/extensionpackage-permission.png


Secured extension API
--------------------------------------------------

The API of extensions can limit the access to certain endpoints by using the **RequiredPermissionRoles** attribute

.. code-block:: C#

    [HttpGet]
    [Route("api/settings")]
    [RequiredPermissionRoles(""AdminSettingsPermissionsPage.Admin"", 
        PermissionScopes.Tenant)]
    public ApiOperationResult<Settings> GetSettings()
    { }



