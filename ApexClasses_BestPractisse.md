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


