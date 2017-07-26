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
		action.setCallback(this, function(response) 
        {
            var result = response.getReturnValue();
        });
        // $A.enqueueAction adds the server-side action to the queue.
		$A.enqueueAction(action);
	}
```

