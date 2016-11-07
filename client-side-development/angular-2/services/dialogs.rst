Dialogs Service
=============================

.. note:: This documentation is a work in progress and contributions can be made on our Github repo

Dialogs service is a utility for working with modal dialogs in Angular 2 and Omnia.

Available Methods
--------------------------------------------------

+----------------------+-------------------------------+--------------------------------------------------------------------------------------------+
| Method               | Description                   | Parameters                                                                                 |
+======================+===============================+============================================================================================+
| onConfirmationDialog | | Show a confirmation dialog  | | - **title (string)**: The dialog's title                                                 |
|                      | | with yes/no outcome         | |                                                                                          |
|                      |                               | | - **body (string)**: The dialog's body text                                              |
|                      |                               | |                                                                                          |
|                      |                               | | - **viewContainerRef (ViewContainerRef)**: the viewContainerRef of the calling component |
|                      |                               | |                                                                                          |
|                      |                               | | - **okCallback (() => void)**: The callback for 'OK' outcome                             |
|                      |                               | |                                                                                          |
|                      |                               | | - **cancelCallback (() => void)**: The callback for 'Cancel' outcome                     |
|                      |                               |                                                                                            |
+----------------------+-------------------------------+--------------------------------------------------------------------------------------------+
| openDialog           | | Show a custom dialog        | | - **componentType (Type<any>)**: The component to show in the dialog                     |
|                      |                               | |                                                                                          |
|                      |                               | | - **params (any)**: the parameters passed to the dialog                                  |
|                      |                               | |                                                                                          |
|                      |                               | | - **viewContainerRef (ViewContainerRef)**: the viewContainerRef of the calling component |
|                      |                               | |                                                                                          |
|                      |                               | | - **dialogSize (DialogSize)**: can be small, medium and large. Default value is medium   |
|                      |                               | |                                                                                          |
|                      |                               | | - **okCallback (() => void)**: The callback for 'OK' outcome                             |
|                      |                               | |                                                                                          |
|                      |                               | | - **cancelCallback (() => void)**: The callback for 'Cancel' outcome                     |
|                      |                               |                                                                                            |
+----------------------+-------------------------------+--------------------------------------------------------------------------------------------+
| blockUI              | | Show a loading indicator    |                                                                                            |
|                      | | and block the whole page UI |                                                                                            |
+----------------------+-------------------------------+--------------------------------------------------------------------------------------------+
| unblockUI            | | Remove the UI block from    |                                                                                            |
|                      | | blockUI method              |                                                                                            |
+----------------------+-------------------------------+--------------------------------------------------------------------------------------------+

Examples
--------------------------------------------------

.. note:: To use the DialogService, you need to import The module OmniaExtensibilityModule into the NgModule of your component or add it directly to the list of providers of your compoment

**Injection**

.. code-block:: javascript

   import { DialogService } from "Omnia/Foundation/Extensibility/Services";
   import { Component, Inject, ViewContainerRef } from '@angular/core';

   @Component({
        selector: 'my-component',
        providers: [ DialogService ]
   })
   export class MyComponent {
        constructor(@Inject(ViewContainerRef) private viewContainer: ViewContainerRef,
                    @Inject(DialogService) private dialogService: DialogService) {
        }
   }
   
**Open confirmation dialog**

.. code-block:: javascript

   private deleteItem(item) {
        let dialogTitle = "Delete Item";
        let dialogBody = "Are you sure you want to delete this item?";

        this.dialogService.onConfirmationDialog(dialogTitle, dialogBody, this.viewContainer, () => {
            // Confirmed, proceed to delete the item
        });
   }

**Open custom dialog**

.. code-block:: javascript

   import { EditItemForm }  from "MyComponent/EditItemForm";
   import { DialogSize } from "Omnia/Foundation/Extensibility/Enums";

   // ...

   private editItem(item) {
        this.dialogService.openDialog(EditItemForm, { item: item }, 
                                      this.viewContainer, DialogSize.Large);
   }   

.. code-block:: javascript

   import { Component, Inject, ViewContainerRef, OnDestroy , OnInit } from '@angular/core';
   import { DialogRef} from 'angular2-modal';
   import { BaseDialogComponent, BaseDialogModel } from "Omnia/Foundation/Extensibility/Services";

   @Component({
        selector: 'edit-item-form'
   })
   export class EditItemForm extends BaseDialogComponent<BaseDialogModel<any>> implements OnInit {
        item: Item;

        constructor(@Inject(DialogRef) public dialog: DialogRef<BaseDialogModel<any>>) {
            super(dialog);
        }

        ngOnInit() {
            this.item = this.context.params.item;
        }        
   }

         