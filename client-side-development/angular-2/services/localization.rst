Localization Service
=============================

.. note:: This documentation is a work in progress and contributions can be made on our Github repo

Localization service is a utility for getting localized text in Omnia. Read more about localization in Omnia `here </fundamentals/localization.html>`_

Available Methods
--------------------------------------------------

+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| Method                    | Description                   | Parameters                                                                                                                                       |
+===========================+===============================+==================================================================================================================================================+
| getText                   | | Get the localized text      | | - **key (string)**: The key for the localization. If no localized text match, this key will be return                                          |
|                           | | for a label in Omnia        |                                                                                                                                                  |
|                           |                               |                                                                                                                                                  |
+---------------------------+-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------------------------------------------------

.. note:: To use the LocalizationService, you need to import The module OmniaExtensibilityModule into the NgModule of your component or add it directly to the list of providers of your compoment

**Injection**

.. code-block:: javascript

   import { LocalizationService } from "Omnia/Foundation/Extensibility/Services";
   import { Component, Inject, ViewContainerRef } from '@angular/core';

   @Component({
        selector: 'my-component',
        providers: [ ConfigurationService ]
   })
   export class MyComponent {
        constructor(@Inject(ViewContainerRef) private viewContainer: ViewContainerRef,
                    @Inject(LocalizationService) private localizationService: LocalizationService) {
        }
   }
   
**Get localized text**

.. code-block:: javascript

   private getItemTypes() {
        return [
            { id: 0, title: this.localizationService.getText('MyExtension.ItemTypes.Small') },
            { id: 1, title: this.localizationService.getText('MyExtension.ItemTypes.Medium') },
            { id: 2, title: this.localizationService.getText('MyExtension.ItemTypes.Large') }        
        ];
   }