### Exception Handler Basics

Errors and exceptions must be handled in a way that provides the user with information regarding what went wrong AND what they should do about it. Whenever possible, this error should also be logged, so the user can provide detailed information regarding the nature of the issue he or she encountered.

The Exception Logging Helper class should be used to log exceptions.
Exception handlers should trap typed exceptions where possible:
```sh
Try
{
//do something here that could throw exception
}
catch (DMLException e)
{
//log then do something
}
catch (Exception e)
{
//log then do something else
}
finally
{
// cleanup
}
```
Do not return from a Finally block as you may lose exceptions in the process:

```sh
public String foo()
{
try
{
throw new Exception( "My Exception" );
}
catch (Exception e)
{
throw e;
}
finally
{
return "A. O. K."; // return not recommended here
}
}
```
1. Exceptions should never be suppressed without a comment in code explaining why 
2. Empty exception handlers are never acceptable. 
3. Exceptions should be logged to a logging object: ApplicationLog__c 
4. Bulk log messages should be collated and passed to the Log in one call to prevent excessive DML calls 
5. Page based exceptions should be added to ApexPages.Messages 
6. In triggers, exceptions should use addError() method to prevent committing 
7. Do not catch NullPointerExceptions – identify and resolve the core issue. 
### ExceptionLoggingHelper

The Exception Logging Helper is used as follows:
- ExceptionLoggingHelper.createErrorLog(User, Class Name, Method Name, Message,EXECEPTION OBJECT, DebugLevel, IntegrationPayload,  ReferenceInfo, Timer);

| Field | Type | Description|
| ------ | ------ |-------|
| User | Id |Current User|
|Class Name|	String|		Class name where the exception occurred|
|Method Name|String		|Method name where the exception occurred
|Message|	String	|	System message or custom message
|Exception|	String	|	Exception Object (to be kept for future changes)
|DebugLevel|	String|		This will be set to ERROR by default; developer can set it either of Error, Info, Warning or Debug|
|Integration Payload|	String|		This will be set to NULL by default; developer needs to set the value in case of integration exception|
|ReferenceInfo	|String|	This will be set to NULL by default; developer needs to set the valuebatch job id in case of any batch jobs|
|Timer|	Number|	This will be set to NULL by default; developer needs to set by calculating execution end time – execution start time|

### Apex Global Variables

Using Global Variables such as $Page, $Action etc allows Salesforce to determine references and prevent deletion of referenced objects.

For example, use:
```sh
<apex:outputLink value="{!$Page.otherPage}">
```

Instead of:
`<a href=”/apex/otherPage”>Go here</a>`

### SOQL inside loops

There is a Governor limit to the number of SOQL queries in an execution context. Ensure that SOQL is executed outside a loop as much as possible to prevent this limit being breached:

Do not do:
```sh
for (Account a : [SELECT Id, Name FROM Account WHERE Name LIKE 'Acme%'])
{
// Your code without DML statements here a.Name=’new name’;
update a;
}
```
Instead do:
```sh
List<Account> accts : [SELECT Id, Name FROM Account WHERE Name LIKE 'Acme%'];
For(Account a: accts)
{
a.name=’new name’;
}
update accts;
}
```

Similarly, design triggers to support bulk invocation.  

Do not do:
```sh
trigger contactTest on Contact (before insert, before update)
{
for(Contact ct: Trigger.new)
{
Account acct = [select id, name from Account where Id=:ct.AccountId];
if(acct.BillingState=='CA')
{
System.debug(‘do something’);
}
}
}
```

Instead do:
```sh
trigger contactTest on Contact (before insert, before update)
{
Set<Id> accountIds = new Set<Id>(); for(Contact ct: Trigger.new)
{
accountIds.add(ct.AccountId);
}
//Do SOQL Query outside the iterator
Map<Id, Account> accounts = new Map<Id, Account>(
[select id, name, billingState from Account where id in :accountIds]);
for(Contact ct: Trigger.new)
{
if(accounts.get(ct.AccountId).BillingState=='CA')
{
System.debug(‘do something');
}
}
}
```
### Querying Large Data sets

If returning a large set of queries causes you to exceed your heap limit, then a SOQL query for loop must be used instead. It can process multiple batches of records through the use of internal calls to query and queryMore.

Instead of:
```sh
Account[] accts = [SELECT id FROM account]; For (Account acct : accts)
{
//Do Something
}
```
Use:
```sh
for (List<Account> acct : [SELECT id, name FROM account])
{
 //Statement
}
```
### Use @future appropriately
It is critical to write your Apex code to efficiently handle bulk or many records at a time. This is also true for asynchronous Apex methods (those annotated with the @future keyword). Even though Apex written within an asynchronous method gets its own independent set of higher governor limits, it still has governor limits. 

Additionally, no more than ten @future methods can be invoked within a single Apex transaction. 
The parameters specified must be primitive data types, arrays of primitive data types, or collections of primitive data types.

Methods with the future annotation cannot take sObjects or objects as arguments. Methods with the future annotation cannot be used in Visual force controllers in either getMethodName or setMethodName methods, nor in the constructor.
Here is a list of governor limits specific to the @future annotation: 
1.	No more than 10 method calls per Apex invocation 
2.	No more than 200 method calls per Salesforce license per 24 hours 

It's important to make sure that the asynchronous methods are invoked in an efficient manner and that the code in the methods is efficient. In the following example, the Apex trigger invokes an asynchronous method for each Account record it wants to process: 
```sh
{
  for(Account a: Trigger.new){
  // Invoke the @future method for each Account
  // This is inefficient and will easily exceed the governor limit of 
  // at most 10 @future invocation per Apex transaction
  asyncApex.processAccount((String)a.id);
  }     
}
```
Here is the Apex class that defines the @future method:

```sh
global class asyncApex {

    @future 
     public static void processAccount(Id accountId) {
       List<Contact> contacts = [select id, salutation, firstname, lastname, email 
                                 from Contact where accountId = :accountId];
 	   
       for(Contact c: contacts){
 	   System.debug('Contact Id[' + c.Id + '], FirstName[' + c.firstname + '], LastName[' + c.lastname +']');
 	   c.Description=c.salutation + ' ' + c.firstName + ' ' + c.lastname;
       }
        update contacts;        
     }   
    }
```

Since the @future method is invoked within for loop, it will be called N-times (depending on the number of accounts being processed). So if there are more than ten accounts, this code will throw an exception for exceeding a governor limit of only ten @future invocations per Apex transaction. 

Instead, the @future method should be invoked with a batch of records so that it is only invoked once for all records it needs to process: 
```sh
    {
    //By passing the @future method a set of Ids, it only needs to be
    //invoked once to handle all of the data. 
    asyncApex.processAccount(Trigger.newMap.keySet());
    }
```

And now the @future method is designed to receive a set of records:
```sh
global class asyncApex {
 
 @future 
 public static void processAccount(Set<Id> accountIds) {
 List<Contact> contacts = [select id, salutation, firstname, lastname, email from Contact 
 								where accountId IN :accountIds];
 for(Contact c: contacts){
   System.debug('Contact Id[' + c.Id + '], FirstName[' + c.firstname + '], LastName[' + c.lastname +']');
   c.Description=c.salutation + ' ' + c.firstName + ' ' + c.lastname;
   }
 update contacts;
 }
}
```

Notice the minor changes to the code to handle a batch of records. It doesn't take a whole lot of code to handle a set of records as compared to a single record, but it's a critical design principle that should persist across all of your Apex code - regardless if it's executing synchronously or asynchronously. 

### Writing Test Methods to Verify Large Datasets

Since Apex code executes in bulk, it is essential to have test scenarios to verify that the Apex being tested is designed to handle large datasets and not just single records. To elaborate, an Apex trigger can be invoked either by a data operation from the user interface or by a data operation from the Force.com Web Services API. The API can send multiple records per batch, leading to the trigger being invoked with several records. Therefore, it is key to have test methods that verify that all Apex code is properly designed to handle larger datasets and that it does not exceed governor limits. 

The example below shows you a poorly written trigger that does not handle bulk properly and therefore hits a governor limit. Later, the trigger is revised to properly handle bulk datasets. 

Here is the poorly written contact trigger. For each contact, the trigger performs a SOQL query to retrieve the related account. The invalid part of this trigger is that the SOQL query is within the for loop and therefore will throw a governor limit exception if more than 100 contacts are inserted/updated. 
```sh
{  
for(Contact ct: Trigger.new){	
Account acct = [select id, name from Account where Id=:ct.AccountId];
if(acct.BillingState=='CA'){
 System.debug('found a contact related to an account in california...');
ct.email = 'test_email@testing.com';
//Apply more logic here....
}
} 
}
```

Here is the test method that tests if this trigger properly handles volume datasets: 
```sh
public class sampleTestMethodCls {
static testMethod void testAccountTrigger(){
		
//First, prepare 200 contacts for the test data
Account acct = new Account(name='test account');
insert acct;	
Contact[] contactsToCreate = new Contact[]{};
for(Integer x=0; x<200;x++){
Contact ct = new Contact(AccountId=acct.Id,lastname='test');
contactsToCreate.add(ct);
}
		
//Now insert data causing an contact trigger to fire. 
Test.startTest();
insert contactsToCreate;
Test.stopTest();	
}}
``` 

This test method creates an array of 200 contacts and inserts them. The insert will, in turn, cause the trigger to fire. When this test method is executed, a System.Exception will be thrown when it hits a governor limit. Since the trigger shown above executes a SOQL query for each contact in the batch, this test method throws the exception 'Too many SOQL queries: 101'. A trigger can only execute at most 20 queries. 

Now let's correct the trigger to properly handle bulk operations. The key to fixing this trigger is to get the SOQL query outside the for loop and only do one SOQL Query:

```sh
{
Set<Id> accountIds = new Set<Id>();
for(Contact ct: Trigger.new)
accountIds.add(ct.AccountId);
//Do SOQL Query	   
Map<Id, Account> accounts = new Map<Id, Account>(
[select id, name, billingState from Account where id in :accountIds]); 
for(Contact ct: Trigger.new){
if(accounts.get(ct.AccountId).BillingState=='CA'){
System.debug('found a contact related to an account in california...');
ct.email = 'test_email@testing.com';
//Apply more logic here....
}
} 
}
```
Note how the SOQL query retrieving the accounts is now done once only. If you re-run the test method shown above, it will now execute successfully with no errors and 100% code coverage.

### Avoid Hard Coding IDs
When deploying Apex code between sandbox and production environments, or installing Force.com AppExchange packages, it is essential to avoid hardcoding IDs in the Apex code. By doing so, if the record IDs change between environments, the logic can dynamically identify the proper data to operate against and not fail. 
Here is a hard coded sample of the record type IDs that are used in an conditional statement. This will work fine in the specific environment in which the code was developed, but if this code were to be installed in a separate org (ie. as part of an AppExchange package), there is no guarantee that the record type identifiers will be the same. 

```sh
for(Account a: Trigger.new){	 
//Error - hardcoded the record type id
if(a.RecordTypeId=='012500000009WAr'){     	  	
//do some logic here.....
}else if(a.RecordTypeId=='0123000000095Km'){
//do some logic here for a different record type...
}
}
```

Now, to properly handle the dynamic nature of the record type IDs, the following example queries for the record types in the code, stores the dataset in a map collection for easy retrieval, and ultimately avoids any hard coding. 

```sh
//Query for the Account record types
List<RecordType> rtypes = [Select Name, Id From RecordType 
where sObjectType='Account' and isActive=true];
//Create a map between the Record Type Name and Id for easy retrieval
Map<String,String> accountRecordTypes = new Map<String,String>{};
for(RecordType rt: rtypes)
accountRecordTypes.put(rt.Name,rt.Id);  
for(Account a: Trigger.new){    	 
//Use the Map collection to dynamically retrieve the Record Type Id
//Avoid hardcoding Ids in the Apex code
if(a.RecordTypeId==accountRecordTypes.get('Healthcare')){     	  	
//do some logic here.....
}else if(a.RecordTypeId==accountRecordTypes.get('High Tech')){
//do some logic here for a different record type...
}   	 
}
```

By ensuring no IDs are stored in the Apex code, you are making the code much more dynamic and flexible - and ensuring that it can be deployed safely to different environments
