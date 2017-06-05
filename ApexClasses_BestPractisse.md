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







