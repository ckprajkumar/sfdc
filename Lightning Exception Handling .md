### Server Side Errors In Lightning
When you are running Lightning Components and if there is any server side error occur it will not show you error on browser.
***Apex Controller***
```sh
public class DemoExceptionController{
 
    @AuraEnabled
    public static String getServerErrorMessage(){
        //DML Exception
        contact con = new contact();
        insert con;
         
        return 'anything';
    }
}
```
***ExceptionDemo.cmp***
```sh
<aura:component controller="DemoExceptionController">
    <aura:attribute name="message" type="String"/>
    <ui:button label="Show Error" press="{!c.showError}"/>
    <br/>
    Error Message :<ui:outputText value="{!v.message}"/>
</aura:component>
```
***ExceptionDemoController.js***
```sh
({
    showError : function(component, event, helper) {
        var action = component.get("c.getServerErrorMessage");
         
        action.setCallback(this, function(a) {
            component.set("v.message", a.getReturnValue());
        });
        $A.enqueueAction(action);
    }
})
```
When you click on “Show Error” button a DML exception will occur on server. This error should be shown on browser by system but it is not shown. I modified ExceptionDemoController.js to manually show server side errors.
```sh
({
    showError : function(component, event, helper) {
        var action = component.get("c.getServerErrorMessage");
        action.setCallback(this, function(a) {
            component.set("v.message", a.getReturnValue());
            if (a.getState() === "SUCCESS") {
                component.set("v.message", a.getReturnValue());
            } else if (a.getState() === "ERROR"){
                console.log(a.getError());
                var errors = a.getError();
                if(errors[0] && errors[0].pageErrors)
                    component.set("v.message", errors[0].pageErrors[0].message);    
            }
        });
        $A.enqueueAction(action);
    }
})
```
Now when you click on “Show Error” button it will show you user friendly error message.
Now modify Apex Controller and ExceptionDemoController.js to show different type of error.

***Apex Controller***
```sh
public class DemoExceptionController{
 
    @AuraEnabled
    public static String getServerErrorMessage(){
        //List Exception        
        List<Integer> li = new List<Integer>();
        li.add(15);
        Integer i2 = li[1]; // Causes a ListException
    }
}
```
***ExceptionDemoController.js***
```sh
({
    showError : function(component, event, helper) {
        var action = component.get("c.getServerErrorMessage");
         
        action.setCallback(this, function(a) {
            component.set("v.message", a.getReturnValue());
             
            if (a.getState() === "SUCCESS") {
                component.set("v.message", a.getReturnValue());
            } else if (a.getState() === "ERROR"){
                console.log(a.getError());
                var errors = a.getError();
                if(errors[0] && errors[0].message)// To show other type of exceptions
                    component.set("v.message", errors[0].message);
                if(errors[0] && errors[0].pageErrors) // To show DML exceptions
                    component.set("v.message", errors[0].pageErrors[0].message);    
            }
        });
        $A.enqueueAction(action);
    }
})
```
### Unrecoverable Errors
The basics of throwing an unrecoverable error in a JavaScript controller.
```sh
<!--c:unrecoverableError-->
<aura:component>
    <lightning:button label="throw error" onclick="{!c.throwError}"/>
</aura:component>
```
Here is the client-side controller source.
```sh
***unrecoverableErrorController.js***
({
    throwError : function(component, event){
        throw new Error("I can’t go on. This is the end.");
    }
}) 
```
### Recoverable Errors
To handle recoverable errors, use a component, such as ui:message, to tell users about the problem.
This sample shows you the basics of throwing and catching a recoverable error in a JavaScript controller.
```sh
<!--c:recoverableError-->
<aura:component>
    <p>Click the button to trigger the controller to throw an error.</p>
    <div aura:id="div1"></div>
    <lightning:button label="Throw an Error" onclick="{!c.throwErrorForKicks}"/>
</aura:component>
```
Here is the client-side controller source.
**recoverableErrorController.js**
```sh
({
    throwErrorForKicks: function(cmp) {
        // this sample always throws an error to demo try/catch
        var hasPerm = false;
        try {
            if (!hasPerm) {
                throw new Error("You don't have permission to edit this record.");
            }
        }
        catch (e) {
            $A.createComponents([
                ["ui:message",{
                    "title" : "Sample Thrown Error",
                    "severity" : "error",
                }],
                ["ui:outputText",{
                    "value" : e.message
                }]
                ],
                function(components, status, errorMessage){
                    if (status === "SUCCESS") {
                        var message = components[0];
                        var outputText = components[1];
                        // set the body of the ui:message to be the ui:outputText
                        message.set("v.body", outputText);
                        var div1 = cmp.find("div1");
                        // Replace div body with the dynamic component
                        div1.set("v.body", message);
                    }
                    else if (status === "INCOMPLETE") {
                        console.log("No response from server or client is offline.")
                        // Show offline error
                    }
                    else if (status === "ERROR") {
                        console.log("Error: " + errorMessage);
                        // Show error message
                    }
                }
            );
        }
    }
})
```