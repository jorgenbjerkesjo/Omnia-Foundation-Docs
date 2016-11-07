Configuration Service
=============================

.. note:: This documentation is a work in progress and contributions can be made on our Github repo

Configuration service is a utility for working with Omnia configurations. Read more about configuration in Omnia `here </fundamentals/configuration.html>`_

Available Methods
--------------------------------------------------

+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| Method                    | Description                   | Parameters                                                                                                                                       |
+===========================+===============================+==================================================================================================================================================+
| getConfiguration          | | Get configuration from      | | - **callback ((result: Configurations.IConfiguration) => void)**: The callback with the requested configuration                                |
|                           | | Omnia Foundation API        | |                                                                                                                                                |
|                           |                               | | - **name (string)**: The name of the configuration                                                                                             |
|                           |                               | |                                                                                                                                                |
|                           |                               | | - **region (string)**: The region of the configuration                                                                                         |
|                           |                               | |                                                                                                                                                |
|                           |                               | | - **extensionPackageId (string) (optional)**: The ID of the extension that created the configuration. Default value is built-in configurations |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| getClientConfiguration    | | Get configuration included  | | - **name (string)**: The name of the configuration                                                                                             |
|                           | | in client-side              | |                                                                                                                                                |
|                           |                               | | - **region (string)**: The region of the configuration                                                                                         |
|                           |                               | |                                                                                                                                                |
|                           |                               | | - **extensionPackageId (string) (optional)**: The ID of the extension that created the configuration. Default value is built-in configurations |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| addOrUpdateConfigurations | | Add or update a list of     | | - **configurations (Array<Configurations.IConfiguration>)**: The list of configurations to add or update                                       |
|                           | | configurations              | |                                                                                                                                                |
|                           |                               | | - **callback ((isSuccess: boolean) => void)**: The callback function                                                                           |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| getConfigurationsInRegion | | Get all configurations by   | | - **region (string)**: The region of the configurations                                                                                        |
|                           | | region                      | |                                                                                                                                                |
|                           |                               | | - **callback ((result: Array<Configurations.IConfiguration>) => void)**: The callback function with result                                     |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| updateConfiguration       | | Update a configuration      | | - **configuration (Configurations.IConfiguration)**: The configuration to be updated                                                           |
|                           |                               | |                                                                                                                                                |
|                           |                               | | - **callback ((isSuccess: boolean) => void)**: The callback function                                                                           |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| deleteConfiguration       | | Remove a configuration      | | - **name (string)**: The name of the configuration                                                                                             |
|                           |                               | |                                                                                                                                                |
|                           |                               | | - **region (string)**: The region of the configuration                                                                                         |
|                           |                               | |                                                                                                                                                |
|                           |                               | | - **callback ((isSuccess: boolean) => void)**: The callback function                                                                           |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+


Examples
--------------------------------------------------

.. note:: To use the ConfigurationService, you need to import The module OmniaExtensibilityModule into the NgModule of your component or add it directly to the list of providers of your compoment

**Injection**

.. code-block:: javascript

   import { ConfigurationService } from "Omnia/Foundation/Extensibility/Services";
   import { Component, Inject, ViewContainerRef } from '@angular/core';

   @Component({
        selector: 'my-component',
        providers: [ ConfigurationService ]
   })
   export class MyComponent {
        constructor(@Inject(ViewContainerRef) private viewContainer: ViewContainerRef,
                    @Inject(ConfigurationService) private configurationService: ConfigurationService) {
        }
   }
   
**Get configuration**

.. code-block:: javascript

   private getDefaultColors() {        
        this.configurationService.getConfiguration((configuration: Configurations.IConfiguration) => {
            let defaultColors = configuration.value;
        }, "defaultcolors", "");
   }