### Server Side Errors In Lightning
When you are running Lightning Components and if there is any server side error occur it will not show you error on browser.
Apex Controller
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
ExceptionDemo.cmp
```sh
<aura:component controller="DemoExceptionController">
    <aura:attribute name="message" type="String"/>
    <ui:button label="Show Error" press="{!c.showError}"/>
    <br/>
    Error Message :<ui:outputText value="{!v.message}"/>
</aura:component>
```
ExceptionDemoController.js
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
