# Salesforce  Do’s covers the following areas:
### Configuration                      
- Custom Fields
- Record Types
- Page Layouts
- Validation Rules
- Workflow Rules

### Code
- Apex Triggers
- Apex Classes
- Unit Test Classes

# Salesforce Do’s - Configuration

|Area   |Best Practice Recommendation  | Cool  |
| ------------- |:-------------:| -----:|
| Custom Objects| Use naming conventions for Custom Object APIs to identify which Customer is using  the object.This helps Orgs housing multiple business units to quickly identify who is using each custom object|Medium|
|  Custom Objects |Include Descriptions for each custom object identifying its purpose  |Medium|
|Custom Fields|Use naming conventions for all custom field APIs to identify which “group” created the field.This allows orgs with multiple business units to easily identify what “belongs” to another group, reducing the risk of other groups making changes that could negatively impact others in the org|High|
|Custom Fields|Fields Maintained by integrations. Indicate in the description text the source of the data|High|
|Custom Fields|Read Only Fields.If the data comes from an external system identify the system in the Help Text If the data is updated on another record, indicate in the help text where |High|
|Record Types|Use a naming convention that identifies which group created/uses the record type and add descriptions for each record type.Easily identifies who uses each record type|Medium|
|Page Layouts|Use naming conventions that identify which group created/uses the layout|Medium|
|Validation Rules| Use Custom Settings to control on/off for all validation rules. Hierarchy based custom settings to control by profile It allows for validation rules to be enforced for some groups (profiles) and not others Allows for quickly turning off validation rules for data loading purposes.|High|
|Workflow Rules|Use Custom Settings to control on/off or all workflow rules.Hierarchy based custom settings to control by profile.Allows for workflows to fire only for select groups (profiles).Allows for quickly turning off workflows for data loading purposes|High|
|Apex Trigger|Only Have One Trigger Per Object. In order to have control on how the code is executed and also minimize the number of queries performed there should only ever be one trigger written per object.|High|
|Apex Trigger|Ensure That Triggers Process Batches Triggers should always ensure batch processes are supported.|High|
|Apex Trigger|Write Trigger Code in Classes Trigger code written directly into the trigger element quickly becomes difficult to maintain when the amount of code exceeds 20 lines of script.|High|
|Apex Trigger|Trigger Logic At the beginning of all trigger code there should be a line that checks the custom setting to see if it should proceed for the user.|High|

Images

#### APEX Trigger - Only Have One Trigger Per Object

In order to have control on how the code is executed and also minimize the number of queries performed there should only ever be one trigger written per object.

```
trigger OpportunityTrigger on Opportunity (before insert, before update, after insert, after update, after delete, after undelete) {
OpportuntiyTriggerHandlerNew  handler = new OpportuntiyTriggerHandlerNew();
OB_TriggerSettings__c settings = OB_TriggerSettings__c.getInstance();
if(settings != null && settings.OB_OPPORTUNITYTriggerEnabled__c ){
    
            if(Trigger.isBefore){
                if(trigger.isInsert){
                    handler.OnBeforeInsert(trigger.new);
                }
                
                if(trigger.isUpdate){
                    handler.OnBeforeUpdate(trigger.new,trigger.oldMap);
                }
            }
            
            if(Trigger.isAfter){
                if(trigger.isInsert){
                    handler.OnAfterInsert(trigger.new,trigger.oldMap);
                }  
                if(trigger.isUpdate){
                    handler.OnAfterUpdate(trigger.new,trigger.old,trigger.oldMap);
                }   
            }
            
        }
}
```
##### APEX Trigger - Ensure That Triggers Process Batches
Triggers should always ensure batch processes are supported
```
trigger accountTestTrggr on Account (before insert, before update) {

    List<String> accountNames = new List<String>{};

    //Loop through all records in the Trigger.new collection
    for(Account a: Trigger.new) {
        //Concatenate the Name and billingState into the Description field
        a.Description = a.Name + ':' + a.BillingState;
    }
}      
}
```
### APEX Trigger - Write Trigger Code in Classes
Trigger code written directly into the trigger element quickly becomes difficult to maintain when the amount of code exceeds 20 lines of script.
public class OpportuntiyTriggerHandlerNew {      public class OpportuntiyTriggerHandlerNew {        List<Opportunity> oppList = new List<Opportunity>();    List<Opportunity> OpptyList = new List<Opportunity>();    List<Opportunity> OppProducerList = new List<Opportunity>();    public static Boolean isFirstTime = true;    Map<Id,Profile> chatterEnabledProfileMap = new Map<Id,Profile>();    List<PermissionSetAssignment> chatterPermAssgmtList = new List<PermissionSetAssignment>();        static List<Messaging.SingleEmailMessage> allMails = new List<Messaging.SingleEmailMessage>();    static List<ConnectApi.BatchInput> underwriterBatchFeeds = new List<ConnectApi.BatchInput>();    static List<ConnectApi.BatchInput> saleSupportBatchFeeds = new List<ConnectApi.BatchInput>();        //get the settings instance    OB_TriggerSettings__c settings = OB_TriggerSettings__c.getInstance();
## APEX Trigger - Write Trigger Code in Classes…. continued 

