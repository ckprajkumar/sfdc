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
1.	Exceptions should never be suppressed without a comment in code explaining why 
2.	Empty exception handlers are never acceptable. 
3.	Exceptions should be logged to a logging object: ApplicationLog__c 
4.	Bulk log messages should be collated and passed to the Log in one call to prevent excessive DML calls 
5.	Page based exceptions should be added to ApexPages.Messages 
6.	In triggers, exceptions should use addError() method to prevent committing 
7.	Do not catch NullPointerExceptions – identify and resolve the core issue. 
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

instead of:
`<a href=”/apex/otherPage”>Go here</a>`
### SOQL inside loops

There is a Governor limit to the number of SOQL queries in an execution context. Ensure that SOQL is executed outside a loop as much as possible to prevent this limit being breached:

Do not do:

for (Account a : [SELECT Id, Name FROM Account WHERE Name LIKE 'Acme%'])
{
// Your code without DML statements here a.Name=’new name’;
update a;
}

instead do:
```sh
List<Account> accts : [SELECT Id, Name FROM Account WHERE Name LIKE 'Acme%'];
For(Account a: accts)
{
a.name=’new name’;
}
update accts;
}
```
Similarly, design triggers to support bulk invocation.  Do not do:

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

Account[] accts = [SELECT id FROM account]; For (Account acct : accts)
{
//Do Something
}

Use:
```sh
for (List<Account> acct : [SELECT id, name FROM account])
{
 //Do Something
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



