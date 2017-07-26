# Lightning Components Best Practices
*	Caching data at the client side can significantly reduce the number of server round-trips and improve the performance of your Lightning components.
*	Access server data using either server actions or the Lightning Data Service
*	Server actions support optional client-side caching, and the Lightning Data Service is built on top of a sophisticated client caching mechanism
### Apex Server-Side Controller
```sh
public with sharing class SimpleSeverSideController 
{
	//Use @AuraEnabled to enable client and server side-access to the method
	@AuraEnabled
    public static string serverEcho(string firstName)
    {
        return ('Hello from the server,'+firstName);
    }
}
```
*	Methods must be static and marked public or global. Non-static methods aren’t supported.
*	If a method returns an object, instance methods that retrieve the value of the object’s instance field must be public.
*	Use unique names for client-side and server-side actions in a component. A JavaScript function (client-side action) with the same name as an Apex method (server-side action) can lead to hard-to-debug issues. In debug mode, the framework logs a browser console warning about the clashing client-side and server-side action names.
#### Do's
 ```sh
doInit : function(component, event, helper) 
    {
		var action = component.get("c.serverEcho");
		action.setParams({ 
            firstName : component.get("v.firstName") 
        });
		action.setCallback(this, function(response){
            var result = response.getReturnValue();
        });
        // $A.enqueueAction adds the server-side action to the queue.
		$A.enqueueAction(action);
	}
```
#### Do Not
```sh
doInit : function(component, event, helper) 
    {
		var action = component.get("c.serverEcho");
		action.setParams({ 
            firstName : component.get("v.firstName") 
        });
		action.setCallback(this, function(response){
            var result = response.getReturnValue();
        });
    }
```
### Lazy instantiation in your own components
*	You can use ```<aura:if>``` to lazily instantiate parts of the UI (see conditional rendering section).
*	Some components (like ```<lightning:tabset>``` and ```<lightning:tab>```) support lazy instantiation by default.
*	You can also instantiate components programmatically using  ```$A.createComponent();``` however, that approach is generally harder to code, maintain, and debug.
#### Toggling visibility using CSS
```sh
 <div aura:id="error" class="slds-hide">{!v.errorMessage}</div>
    component.find("error").toggleClass(!isError,"slds-hide");
```
or
```sh
<div class="{!v.isError?null:'slds-hide'}">{!v.errorMessage}</div>
```
The ```<div>``` component and all of its children are created and rendered up front, but they’re hidden. If there are event listeners attached to the ```<div>``` or any of its children, these event listeners would be “live.”
#### Conditionally creating elements using ```<aura:if>```
```sh
    <aura:if isTrue="{!v.isError}">
        <div>{!v.errorMessage}</div>
    </aura:if>
```
*	The ```<div>``` component and all its children are only created if the value of the isTrue expression evaluates to true
*	All the components inside the ```<aura:if>``` tag are destroyed if the value of the isTrue expression changes and evaluates to false. They’re re-created the next time the value of the isTrue expression changes again and evaluates to true.

The ```<aura:if>``` approach because it helps your components load faster.
The ```<aura:renderIf>``` approach is deprecated and shouldn’t be used
### Using $Resource in Component Markup
An external CSS resource that you’ve uploaded as a static resource, use a <ltng:require> tag in your .cmp or .app markup.
```sh 
<ltng:require styles="{!Resource.resourceName}"/>
```
Loading Sets of CSS
> Specify a comma-separated list of resources in the styles attribute to load a set of CSS.

```sh
style="{!join(',',$Resource.myStyles+'/stylesheetOne.css',$Resource.myStyles+'/moreStyles.css')}"
```
#### Do's
```sh
<ltng:require scripts="{!join(','
                 $Resource.myResources+'/scripts/js1.js'},
    			 $Resource.myResources+'/scripts/js2.js'}" afterScriptsLoaded="{!c.setup}"/>
```
#### Do Not
```sh
<ltng:require style="/resource/myResources/styles/styles.css"
    scripts="/resource/myResources/scripts/js1.js,/resource/myResources/scripts/js2.js"             afterScriptsLoaded="{!c.setup}"/>
```
### Call multiple helper methods
#### Do’s
 Controller.js
 ```sh
 doInit : function(component, event, helper) 
    {
        helper.method1(component);
        helper.method2(component);
        helper.mwthod3(component);
    }
```
Helper.js
```sh
 ({
        method1:function(component){
        	//do some stuff
        },
 		method2:function(component){
        	//do some stuff
        },
        method3:function(component){
        	//do some stuff
        }    	
	})
```
```sh
({
        method1:function(component){
        	var action = component.get("c.callingmethod1");
    		var self = this;
    		action.setcallback(this,function(resp))
    		{
				//do some stuff and call method1
				self.method1(component);    
			});
			$A.enqeueAction(action);
        },
 		method2:function(component){
        	var action = component.get("c.callingmethod2");
    		var self = this;
    		action.setcallback(this,function(resp))
    		{
				//do some stuff and call method1
				self.method2(component);    
			});
			$A.enqeueAction(action);
        },
        method3:function(component){
        	var action = component.get("c.callingmethod3");
    		var self = this;
    		action.setcallback(this,function(resp))
    		{
				//do some stuff and call method1
				self.method3(component);    
			});
			$A.enqeueAction(action);
        }    	
	})
```

