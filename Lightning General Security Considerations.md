# General Lightning Security Considerations
- When developing Lightning apps, make sure to enable LockerService in your Developer Edition org.
### Content Security Policy for Lightning Components
- Lightning components are currently subject to a Content Security Policy (CSP) with the following directives. See CSP Directives for more information.
![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/Picture9.png?raw=true)
![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/Picture10.png?raw=true)


### Additional Restrictions for JavaScript in Lightning Components
- To change styles dynamically, use $A.util.toggleClass(). Dynamic operations such as fade in and fade out are discouraged in components. If you require them, use CSS animations.
- For loading script or style resources, use the ltng:require component rather than including the resource via a script or link tag.
- Avoid inline JavaScript except when referencing JavaScript controller methods in the component markup.

    <div onmouseover="myfunction" >foo</div>    //bad
    <div onmouseover="{!c.myControllerFunction}" >foo</div>    //ok

### Sharing in Apex Classes
    public class ExpenseController() {    //unsafe
    @AuraEnabled
    public static String getSummary() {
        doPrivilegedOp()    //should not be here
    }
    }
    
    public with sharing class ExpenseController() {    //safe
    @AuraEnabled
    public static String getSummary() {
    HelperClass.doPrivilegedOp()
    }
    }
    
    public without sharing HelperClass() {    //safe, not a controller and limited functionality
    protected static String doPrivilegedOp() {
        //calculate roll - up field here
    }
    }
    

