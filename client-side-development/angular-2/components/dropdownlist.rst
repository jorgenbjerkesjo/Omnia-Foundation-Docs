DropDownList
=============================

.. note:: This documentation is a work in progress and contributions can be made on our Github repo

DropDownList is a dropdown component that supports type-ahead search and customizable UI.


How it looks like
--------------------------------------------------

.. image:: ../../../images/singlepicker.png

Selector
--------------------------------------------------

.. code-block:: html

   <omf-dropdown-list></omf-dropdown-list>

Parameters
--------------------------------------------------

| - **items: Array<any>**
| The list of options for the dropdown. Could be an array of string or an array of object.
|
|

| **allowMultipleValues: boolean**
| Flag for allowing multiple values to be selected. Default value is **false**.
|
|

| **textProperty: string**
| The name of property to be used as the display text of the options. Only applicable if **items** is an array of object.
|
|

| **textExpression: string**
| The expression for formatting the display text of the options. **textProperty** will be ignored if **textExpression** is set. Only applicable if **items** is an array of object.
|
| Example: text-expression="[firstName] [lastName] ([email])"
|
|

| **valueProperty: string**
| The name of the property to be used as the selected value. Only applicable of **items** is an array of object.
| 
|

| **preSelectFirstItem: boolean**
| Flag for setting the first item in **items** as selected if no selected value is provided. Default value is **false**.
|
|

| **onItemSelected: (selectedItem: any, parentItem?: any) => void**
| Event handler for item selected event. If **items** is an array of object, **selectedItem** will be the object that was selected, not just the value property.
|
|

| **onItemDeselected: (deselectedItem: any, parentItem?: any) => void**
| Event handler for item deselected event. If **items** is an array of object, **deselectedItem** will be the object that was selected, not just the value property.
|
|

| **onDropDownOpen: () => void**
| Event handler for dropdown open event.
|
|

| **onInputChange: (newValue: string) => void**
| Event handler for 
|
|

| **showLoading: boolean**
| Flag for showing the loading indicator. Should be set to true while **items** is being loaded. Default value is **false**.
|
|

| **selectedItemValue: any**
| Two-way bind selected value(s). If **items** is an array of object then the selected value(s) will be taken from the **valueProperty** of the selected item.
| If **allowMultipleValues** is true then **selectedItemValue** will be an array, otherwise it will be a single value.
|
|

| **parentItem: any**
| The related object to be passed in **onItemSelected** and **onItemDeselected**.
|
|

Examples
--------------------------------------------------

.. code-block:: javascript
   
   import { Component, Inject, ViewContainerRef } from '@angular/core';

   @Component({
        selector: 'my-component'        
   })
   export class MyComponent {
        employees = [
            { id: 1, firstName: 'Mary', lastName: 'Brown', age: 27 },
            { id: 2, firstName: 'John', lastName: 'Smith', age: 36 }
        ];

        selectedEmployeeId = 1;

        constructor(@Inject(ViewContainerRef) private viewContainer: ViewContainerRef) {
        }
   }

.. code-block:: html

    <omf-dropdown-list [items]="employees" 
                       textProperty="firstName" 
                       valueProperty="id" 
                       [(selectedItemValue)]="selectedEmployeeId">
    </omf-dropdown-list>