AJAX Service
=============================

.. note:: This documentation is a work in progress and contributions can be made on our Github repo

AJAX service is a utility for making AJAX requests to Omnia Foundation and Omnia extensions web API. The AJAX service is usually not used directly in UI components but in other services.

Available Methods
--------------------------------------------------

+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| Method                    | Description                   | Parameters                                                                                                                                       |
+===========================+===============================+==================================================================================================================================================+
| buildRequest              | | Fluent API for making       | | - **apiPath (string)**: The request's URL. If apiPath is a relative URL, it will be direct to Omnia Foundation.                                |
|                           | | an AJAX request             | | Other extensions can inherit this AJAX and override the internal method **getFullApiPath** to make calls to it's API instead                   |
|                           |                               | |                                                                                                                                                |
|                           |                               | | - **dataType (string) (optional)**: The request's Content-Type. Default value is 'application/json'                                            |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------------------------------------------------

.. note:: To use the AjaxService, you need to import The module OmniaExtensibilityModule into the NgModule of your component or add it directly to the list of providers of your compoment

**Normal Usage**

.. code-block:: javascript

   import { AjaxService } from "Omnia/Foundation/Extensibility/Services";
   import { Pipe, Injectable , Inject } from '@angular/core';

   @Injectable()
   export class ConfigurationService {
        constructor(@Inject(AjaxService) private ajaxService: AjaxService) {
                    
        }

        public getConfiguration = (callback: (result: Configurations.IConfiguration) => void, name: string, region: string, extensionPackageId: string = null) => {
            var params = {
                name: name,
                region: region,
                extensionPackageId: extensionPackageId
            };

            this.ajaxService.buildRequest("configuration/configurations")
                .addQueryStrings(params)
                .doGet<Configurations.IConfiguration>()
                .subscribe((result) => { callback(result.json()); });
        }
   }
   
**Inherit AJAX Service**

.. code-block:: javascript

   import { AjaxService as FoundationAjaxService } from 'Omnia/Foundation/Extensibility/Services'
   import { Utils } from "Omnia/Foundation/Extensibility";

   @Injectable()
   export class AjaxService extends FoundationAjaxService{
       static apiBaseUrl: string = "";

       public getFullApiPath(apiPath: string): string {
           if (Utils.isNullOrEmpty(AjaxService.apiBaseUrl)) {
               AjaxService.apiBaseUrl = Utils.ensureTrailingSlash("<my-extension-api-url>");
           }
            
           return AjaxService.apiBaseUrl + apiPath;
       }
   }