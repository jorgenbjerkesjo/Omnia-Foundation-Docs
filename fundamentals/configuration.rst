Configuration
============================

Omnia configuration API provides a key-value storage that can be accessed from both server-side and client-side code. 

A configuration can have the following properties:

+--------------------+------------------------------+-------------------------------------------------------------+
| Property           | Type                         | Description                                                 |
+====================+==============================+=============================================================+
| Name               | String                       | | Name of the configuration (case-insensitive)              |
|                    |                              |                                                             |
+--------------------+------------------------------+-------------------------------------------------------------+
| Region             | String                       | | Group or category of the configuration (case-insensitive) |
|                    |                              |                                                             |
+--------------------+------------------------------+-------------------------------------------------------------+
| ExtensionPackageId | Guid                         | | ID of the extension package that the configuration        |
|                    |                              | | belongs to                                                |
+--------------------+------------------------------+-------------------------------------------------------------+
| Value              | String                       | | Value of the configuration                                |
|                    |                              |                                                             |
+--------------------+------------------------------+-------------------------------------------------------------+
| UIEditable         | Boolean                      | | Specify whether the configuration is editable in          |
|                    |                              | | Omnia admin app                                           |
|                    |                              |                                                             |
+--------------------+------------------------------+-------------------------------------------------------------+
| IncludedInClient   | Boolean                      | | Specify whether the configuration should be included      |
|                    |                              | | in the client-side context in                             |
|                    |                              | | _omniaContextInfo.customConfigurations                    |
|                    |                              |                                                             |
+--------------------+------------------------------+-------------------------------------------------------------+

A configuration "key" is the unique combination of its name, region and extension ID. Configuration value is string-based so it can be anything from plain string to URL or JSON.


Configuration API for server-side code
--------------------------------------------------

.. note:: 

    To use Omnia configuration API in server-side code you need to install the NuGet package **Omnia.Foundation.Extensibility.Core** to you project.

In server-side code, the configuration API can be called by using **OmniaApi** factory

.. code-block:: C#

    IConfigurationService configurationService = OmniaApi.WorkWith(tenantId).Configurations();

When working with configuration API, you can specify the extension ID that the configuration belongs to, or you can omit it. When omitted, the extension package ID will be read from the app setting **Omnia.Foundation.Settings.ExtensionId** of the current app domain.

**Example**: Get a single configuration by name and region

.. code-block:: C#

    // Get a built-in configuration from Omnia Foundation
    OmniaApi.WorkWith(tenantId).Configurations().GetConfiguration(
                name: "configuration-name", 
                region: "configuration-region",
                extensionPackageId: Extensibility.Core.Constants.Extensions.BuiltInExtensionPackageId);

    // Get a configuration from your extension. When omitted, the extension package ID will be read 
    // from the app setting 'Omnia.Foundation.Settings.ExtensionId' of the current app domain
    OmniaApi.WorkWith(tenantId).Configurations().GetConfiguration(
                name: "configuration-name", 
                region: "configuration-region");                    

    // Get a configuration from another extension.
    OmniaApi.WorkWith(tenantId).Configurations().GetConfiguration(
                name: "configuration-name",
                region: "configuration-region",
                extensionPackageId: "extension-id");                    

**Example**: Get all configurations in a region

.. code-block:: C#
    
    OmniaApi.WorkWith(tenantId).Configurations().GetConfigurationsInRegion(
                name: "configuration-region", 
                extensionPackageId: "extension-id");

**Example**: Add or update a configuration

.. code-block:: C#

    MyModel myModel = new MyModel();

    OmniaApi.WorkWith(tenantId).Configurations().AddOrUpdateConfiguration(
                name: "configuration-name",
                value:  JsonConvert.SerializeObject(myModel),
                region: "configuration-region", 
                includedInClient: false,
                uiEditable: false,
                extensionPackageId: "extension-id");

**Example**: Delete a configuration

.. code-block:: C#

    OmniaApi.WorkWith(tenantId).Configurations().DeleteConfiguration(
                name: "configuration-name",
                region: "configuration-region", 
                extensionPackageId: "extension-id");

Configuration for extension
--------------------------------------------------

You can automatically set the configurations for your extension when it is deployed by specifying those configurations in the **extension.json** file

.. code-block:: json

    {
        "Id": "3847fb18-8cb7-4597-83d8-6bcb7136ce7a",
        "Title": "MyOmniaExtension",
        "Description": "",
        "Version": "1.0.0",
        "PackageName": null,  
        "TenantResourceFolders": [
            "TenantResources"
        ],
        "Configurations": [
            {
                "Name": "MyWebApiUrl",
                "Region": "MyOmniaExtension",
                "IncludedInClient": true,
                "UIEditable": true,
                "Required": true,
                "DefaultValue": "https://localhost:44300/api/"
            },
            {
                "Name": "TermSetId",
                "Region": "MyOmniaExtension",
                "IncludedInClient": true,
                "UIEditable": true,
                "Required": true,
                "DefaultValue": "888F3B90-9A90-4F77-B64D-305EFF1EB3D5"
            }
        ]
    }

Configuration default values will be used when the extension package is deployed the first time from Visual Studio. When the package is uploaded from Omnia admin app user will need to fill in the configuration values.

.. image:: /images/omnia-admin-new-extension-upload-configurations.png