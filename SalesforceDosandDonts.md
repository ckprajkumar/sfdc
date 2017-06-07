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

#### Use Custom Settings to control on/off for all Validation Rules
![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/ControlOnOff.jpg)
#### Use Custom Settings to control on/off for all Workflow Rules
![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/Cntlr2.jpg)
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
```

public class OpportuntiyTriggerHandlerNew {
public class OpportuntiyTriggerHandlerNew
{
List<Opportunity> oppList=newList<Opportunity>();
List<Opportunity> OpptyList=newList<Opportunity>();
List<Opportunity> OppProducerList=newList<Opportunity>();
public static Boolean isFirstTime=true;
Map<Id,Profile> chatterEnabledProfileMap = new Map<Id,Profile>();
List<PermissionSetAssignment> chatterPermAssgmtList = new List<PermissionSetAssignment>();
static List<Messaging.SingleEmailMessage> allMails=new List<Messaging.SingleEmailMessage>();
static List<ConnectApi.BatchInput> underwriterBatchFeeds=new List<ConnectApi.BatchInput>();
static List<ConnectApi.BatchInput> saleSupportBatchFeeds= new List<ConnectApi.BatchInput>();

//get the settings instance OB_TriggerSettings__c settings=OB_TriggerSettings__c.getInstance();
```

## APEX Trigger - Write Trigger Code in Classes…. continued 
```
//BEFORE INSERT EVENT 
public void OnBeforeInsert(List<Opportunity> newList){
//check the trigger setting flag is enable

if(settings!=null&&settings.OPP_OpportunityDataLoadTranslations__c){
/*Thismethodwillberesponsiblefor:
*1.)AssigningAppropriate Proposal Type on Opportunity Record.
*2.)Assign Appropriate Opportunity Type on Opportunity Record.
*3.)Assign Appropriate Opportunity Record Type on Opportunity Record.
*4.)Assign Appropriate Opportunity Close Date on Opportunity Record.
*5.)Assign Appropriate Opportunity Stage on Opportunity Record.
*6.)Assigning Appropriate Opportunity Amendment Group on Opportunity Record.
*/
OpportunityDataLoadTranslations(newList);
}
}
```
### APEX Trigger - Trigger Logic
At the beginning of all trigger code there should be a line that checks the custom setting to see if it should proceed for the user.

```
trigger OpportunityTrigger on Opportunity(before insert,before update,after insert,
after update,after delete,after undelete)
{
OpportuntiyTriggerHandlerNewhandler=newOpportuntiyTriggerHandlerNew();
//getthesettingsinstance

OB_TriggerSettings__csettings=OB_TriggerSettings__c.getInstance();

}
```


### APEX Classes - Do Not Expect Queries to Return Single Objects

Store the SOQL result in an array or list.

```
//Approach -  specify a MyCustomSetUpObject__c[] arrayMyCustomSetUpObject__c[]  
myObjArray = [SELECT Id, Name, Setting__c FROM MyCustomSetUpObject__c WHERE Id =: myObjId];
//If the array has entries in it then proceed 
if(myObjArray.size() > 1){myObj  = myObjArray[0];
//Update the settings 
myObj. Setting__c = ‘New Setting Details’;
//Update the object 
try{
       update myObj;
     } Catch (DMLException dml){

    // Invoke the common exception handling class log the exception , also throw a meaniful error message back to users if controller get called from VF or Lightning components
     } Catch ( Exception ex){
   // Invoke the common exception handling class log the exception , also throw a meaniful error message back to users if controller get called from VF or Lightning components  
}
}
```
### APEX Classes - Do Not Hard Code Ids

Ids to specific information in the environment such as a record id type or a specific user profile should not hardcoded into the system.
Leverage Custom Settings or Custom Label.
Store the SOQL result in an array or list.
```
Map<String, AccountEstWorth__c> acctWorthRefTable = AccountEstWorth__c.getAll();

for (Account acct: Trigger.new) {                                                       if(acctWorthRefTable.containsKey(acct.ClassificationCode__c)){
     AccountEstWorth__c aewRec = acctWorthRefTable.get(acct.ClassificationCode__c);      acct.EstimatedWorth__c = aewRec.EstWorth__c;      
     }
}
```

AVOID:

```
if (acct.ClassificationCode__c.equals('AC-099')||acct.ClassificationCode__c.equals('NM-467')) 
{
acct.EstimatedWorth__c = 25000000;
}
```
### APEX Classes - Avoid Hard coding Dynamic Queries
Call getQuery method to return the string used to pass into dynamic queries.
The QueryLocator should be used solely as a utility class to construct the query string from the static query.
```
List<Account> accnts;

//Query 1 return all accounts which are in the gold tier 

string query1String = Database.getQueryLocator([SELECT Id FROM Account WHERE Tier__c = ‘Gold’]).getQuery();

Accnts = DynamicQueryMethod(query1String);
```

AVOID: Query 1 that returns all accounts which are in the gold tier
Accnts = DynamicQueryMethod(‘SELECT Id FROM Account WHERE Tier__c = \‘Gold\’’);

#### APEX Classes -Use Relationship and Aggregate Queries

Relationship queries or subqueries should be used to reduce the number of SOQL calls issued from a customization.

List<Account> acctList = [SELECT Id, Name, Website, (SELECT Id, Name FROM Contacts)FROM Account WHERE (Id IN :acctIDs)];

### APEX Classes -Ensure Exceptions are Handled

![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/APEX%20Classes%20-Ensure%20Exceptions%20are%20Handled.jpg)

### APEX Classes -Ensure Exceptions are Handled
The Technical approach for Exceptions handling has been proposed by keeping Salesforce standards consistent across projects
There are 3 things that needs to be looked at as a part of the Exception Handling: 
1.Exception Handling

2.Debugging

3.Prevention of the Unhand able Exceptions 

Eg: Governor Limits Exceptions.
Technical Approach: 
Exceptional Handling for Pages: 
1: Add <apex:pageMessages/> tag to your page
2: Enclose your DML statement with try catch like this:

```
try{
    //Your code may cause exception 
    insert recordtoUpdate;
}catch(System.Dmlexception e){
    //To show specific Validation Rule Message
    //Specific exception handling code here
    ApexPages.addMessage(newApexPage.Messages.Severity.ERROR,e.getDmlMessage(0));
}catch(System.Typeexception e){
    //Another Specific exception to be shown
    ApexPages.addMessage(newApexPage.Messages.Severity.ERROR,LABEL.IPM_ERR_DATE_FIELD);
}catch(Exception e){
    //Another Specific exception to be shown
    ApexPages.addMessage(newApexPage.Messages.Severity.ERROR,LABEL.IPM_ERR_TECHNICAL_MESSAGE);
}finally{
    //Optional finally block
    //code to run whether there is an exception or not
}
```

### APEX Classes -Ensure Exceptions are Handled
Exceptional Handling for Automation or Asynchronous Calls: 
1. Implement an exception handling in the following way:
```
Database.SaveResults[] lsr = Database.update(accounts,false);
list<Case> caseWithErrors = new List<Case>();
for(Database.SaveResults sr: lsr){
    if(!sr.isSuccess()){
       Case caseObj() = new Case();
       caseObj.Name='Error for IPM';
       caseObj.Description=sr;
       caseWithErrors.add(caseObj);
}
```

##### Custom Exception Handling: 
 Another option is to create Custom exception class and use internal Code error mechanisms to throw specific exceptions.

```
try{
 //some logic
 if(i>5){
    throw IPMException(IPMErrorCode.INVALID_DATA);
    }
 }catch(IPMException e){
 // Display the message to the user
 ApexPages.addmessage(e.getMessage());
 }catch(Exception e){
 //Display the message to the user
 ApexPages.addMessage(e.getMessage());
 }finally{
 //Handle anything after the logic (Transaction handling for example)
 }
```

#### Unit Test Classes - Test Classes For Triggers Should Perform Bulk Tests

Use System.runAs()

```
@IsTest
private class PortalRunAsTests{
enum PortalType{CSPLiteUser,PowerPartner,PowerCustomerSuccess,CustomerSuccess}
static testmethod void usertest(){
User pu= getPortalUser(PortalType.PowerPartner,null,true);
System.assert([select isPortalEnabled from user where id:pu.id].isPortalEnabled,
'User was not flagged as portal enabled.');
}
System.RunAs(pu){
System.assset([Select isPortalEnabled from user where id = :UserInfo.getUserId().isPortalEnabled],'User wasnt portal enabled within runas block');
}
}
```

### Unit Test Classes - Unit Test Classes Should not Depend on Organization Data
Unit test classes should not rely on the existence of any data within the organization.Data should be created at the beginning of all test methods; ideally the data creation will be wrapped up in a shared method in a data factory class stored elsewhere in the application.
```
static testMethod void TestFunctionality()
{ 
//Create products to be associated with order    
Product2 prod1 = new Product2();    
prod1.Name = 'Product1';  
prod1.Price__c = 100;
insert prod1;                                 
//Create account to add to order    
Account accnt1 = new Account();  
accnt1.Name ='Test1 Account';    
insert accnt1;
}
```

###### Use of Test Factory Class
With it you can say:
```
Account a = (Account)TestFactory.createSObject(new Account());
```
```
Opportunity o = (Opportunity)TestFactory.createSObject(new Opportunity(AccountId=a.Id));
```
```
Account[] aList = (Account[])TestFactory.createSOBjectList(new Account[],200);
```
### Avoid Broken Null Check

Avoid Broken Null Check 

Instead of this line:
```
      if(myStringData != null && myStringData != '') {
```
You can use this one:
```
      if(String.isBlank(myStringData) {
```     
      
Instead of the following code:
```
if(accList != null && accList.size() == 0){
```
You can use this one (Less Code, ONE test):
```
     if (!accList.isEmpty()) {
}
```
