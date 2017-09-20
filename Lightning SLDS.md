## SLDS
The Salesforce Lightning Design System (SLDS) component library is actively developed to enable Salesforce developers to create a uniform look and feel across all Salesforce-related applications while adhering to CSS best practices and conventions.
 https://www.lightningdesignsystem.com/
### SLDS Grid
* Small for phone (page width between 480px and 767px)
* Medium for a tablet (page width between 768px and 1023px)
* Large for a laptop or desktop (width above 1024px)
* Default as a fallback category when no other categories apply
```sh
<div class="slds-grid slds-wrap">
    <div class="slds-size--1-of-1">1</div>
    <div class="slds-size--1-of-2 slds-medium-size--5-of-6 slds-large-size--8-of-12">2</div>
    <div class="slds-size--1-of-2 slds-medium-size--1-of-6 slds-large-size--4-of-12">3</div>
    <div class="slds-size--1-of-1 slds-medium-size--1-of-2 slds-large-size--1-of-3">4</div>
    <div class="slds-size--1-of-1 slds-medium-size--1-of-2 slds-large-size--1-of-3">5</div>
    <div class="slds-size--1-of-1 slds-large-size--1-of-3">
        <div class="slds-grid slds-wrap">
            <div class="slds-size--1-of-2 slds-medium-size--1-of-1 slds-large-size--1-of-2">6</div>
            <div class="slds-size--1-of-2 slds-medium-size--1-of-1 slds-large-size--1-of-2">7</div>
        </div>
    </div>
</div>

```
### Responsive Lightning components
```sh
<lightning:layout horizontalAlign="spread" multipleRows="true">
    <lightning:layoutItem flexibility="grow" size="12">1</lightning:layoutItem>
    <lightning:layoutItem flexibility="grow" size="8" mediumDeviceSize="10" largeDeviceSize="8">2</lightning:layoutItem>
    <lightning:layoutItem flexibility="grow" size="4" mediumDeviceSize="2" largeDeviceSize="4">3</lightning:layoutItem>
    <lightning:layoutItem flexibility="grow" size="4" mediumDeviceSize="6" largeDeviceSize="4">4</lightning:layoutItem>
    <lightning:layoutItem flexibility="grow" size="4" mediumDeviceSize="6" largeDeviceSize="4">5</lightning:layoutItem>
    <lightning:layoutItem flexibility="grow" size="12" largeDeviceSize="4">
        <lightning:layout horizontalAlign="spread" multipleRows="true">
            <lightning:layoutItem flexibility="grow" size="6" mediumDeviceSize="12" largeDeviceSize="6">6</lightning:layoutItem>
            <lightning:layoutItem flexibility="grow" size="6" mediumDeviceSize="12" largeDeviceSize="6">7</lightning:layoutItem>
        </lightning:layout>
    </lightning:layoutItem>
</lightning:layout>
```
#### Some examples:
#### Buttons
```sh
<button class="slds-button slds-button_success">Button</button>
```
[Image]

#### Without SLDS 
```sh
<button type="button">Click Me!</button> 
```
### Drop Down
	[Menus - https://www.lightningdesignsystem.com/components/menus/]
	[Picklist - https://www.lightningdesignsystem.com/components/picklist/
### Lookups
	https://www.lightningdesignsystem.com/components/lookups/
### Trees
	https://www.lightningdesignsystem.com/components/trees/
