# 5.3 Apex Classes

It is not legal to define a class and interface with the same name in the same class. It is also not legal for an inner class to have the same name as its outer class. However, methods and variables have their own namespaces within the class so these three types of names do not clash with each other. In particular it is legal for a variable, method, and a class within a class to have the same name.

```sh
/********************************************************************** 
Name: ConfigureOpportunity()
Copyright © 2015  Salesforce
======================================================
======================================================
Purpose:

-------
======================================================
======================================================
History				
-------				
VERSION	AUTHOR	DATE	DETAIL	Description
1.0	Name	dd/mm/yyyy	INITIAL DEVELOPMENT	CSR:
***********************************************************************/
<access> class <name>
{
//Static Block
{
//Static variable //Static Methods
}
//Non-Static Variables
..
//	Constructors 
.. 
//	getter & setter methods 
.. 
//	Action Methods 
.. 
//	Logical Methods 
//	Inner Classes and Functions 
}
```
###### Figure 1 - Apex Class layout

### 5.2. Methods
Methods must have a try-catch-finally block to handle possible exceptions. The try-catch block must appear at logical places where you are sure of Exception nature and its handling.
Methods must have Debug statements in the beginning and at the end or before the return. This must also include Input and Output parameters to help debugging the code (as shown in the example below).Methods must have block comments to capture the details in format shown in Figure 2 -Block comments
```sh
public class ConfigureProducts

{
..
..
/******************************************************************* 
Purpose: Detailed description of the method

Parameters: [optional] Returns: [optional]
Throws [Exceptions]: [optional]
********************************************************************/

public String addUDAC(param1, param2)
{
system.debug(‘Entering <Method Name>: ‘+ <I/P parameters>);
…
system.debug(‘Exiting <Method Name>: ‘+ <return value if any>); return ..;
}
}
```
###### Figure 2 - Block comments
Naming convention for the getter and setter methods of Class variables is "get"/"set" followed by the variable name, with the first letter of the name capitalized.

For example, the instance variable "customerID", can have the following formats for the getter and setter methods:
```sh
public string customerID{get; set;}
public string customerID;	// Variable declaration section
public string getCustomerID()
{  //Get method definition 
return <string>;
}
public string setCustomerID() { //set method definition
return <string>;
}
```

### 5.3.	Static Variable
Never hard code an Id, link etc. in the code directly. Use Custom Labels or static Apex

Classes as a mechanism to drive values for a variable.

### 5.4.	Workflow rules and Triggers
Both Workflow rules and triggers must be able to be prevented from running by changing a custom settings value. This should not be implemented by modifying the User object.

The object specific trigger dispatcher will be augmented to check to see if the specified trigger should be bypassed for the currently logged in user. The execute method will not be called if a match is found.

### 5.5.	Code Blocks
Always use braces to…else,surroundwhile,codefor).blocks (eg if

Never use the pattern:
```sh
if ([condition]) 
// statements
```
Always use:
```sh
if ([condition])
{
//statement 1
}
else
{
//statement 2
}
```
Never leave empty code blocks:
```sh
if([condition]) { } else {
// statement
}
```

## 5.6.	Class length
Excessive class file lengths are usually indications that the class may be burdened with excessive responsibilities that could be provided by external classes or functions. In breaking these methods apart, the code becomes more manageable and ripe for reuse.


Try to reduce the method length by creating helper methods and removing any copy/pasted code.

## 5.7.	Method length
Avoid long methods and classes – if necessary refactor the code.

When methods are excessively long this usually indicates that the method is doing more than its name/signature might suggest. They also become challenging for others to digest since excessive scrolling causes readers to lose focus.

## 5.8.	Parameter lists
Methods with numerous parameters are a challenge to maintain, especially if most of them share the same datatype. These situations usually denote the need for new objects to wrap the numerous parameters.

## 5.9.	Excessive Fields
Classes that have too many fields can become unwieldy and could be redesigned to have fewer fields, possibly through grouping related fields in new objects.

For example, consider:
```sh
public	class Person {	// many separate fields
int	birthYear;	
int	birthMonth;	
int	birthDate;	
float height;	
float weight;	
}		
```
```sh
public	class Person {	// more manageable
Date birthDate;	
BodyMeasurements measurements;
}		
```

## 5.10.   String Literals
Code containing string literals can usually be improved by declaring the String as a constant for ease of maintenance – particularly where it is duplicated. The exception here is for

SOQL statements.

## 5.11.   String Conversion
Do not convert literals to strings by concatenating them with strings. Use one of the type-specific toString() methods instead.
```sh
String	s	=	"" + 123;	// inefficient	
String	t	=	Integer.toString(456);	// preferred approach	
```

## 5.12.   If Statements
Avoid creating deeply nested if-then statements since they are harder to read and error-prone to maintain.
```sh
if (x>y) {
if (y>z) {
if (z==x) {
//  too deep
}
}
}
}
```

## 5.13.   Exceptions
Do not throw raw exceptions.  Throw either standard typed or custom exceptions
```sh
//define custom exception

public class MyException extends Exception{}
try
{
throw new MyException();
}
catch ( MyException e )
{
//MyException handling code here
}
```
Do not throw NullPointerException – developers may misunderstand the source – instead consider   throwing an InvalidParameterValueException.

Avoid rethrowing an existing exception, or wrapping it in another exception – either don’t catch it, or handle it correctly:
```sh
try
{
// statements
}
catch (SomeException se)
{
throw se;  // incorrect statement
throw new SomeException(se); //nor this
}
```
Avoid throwing exceptions in a finally block as it can hide other exceptions:
```sh
try
{
// statements
}
catch( Exception e)
{
// Handling the issue
}
finally
{
//Avoid doing this throw new exception()
}
```
Don’t needlessly access Exception values unless you’re going to use them. Don’t do this:
```sh
try {
// statements
} catch (SomeException se) { se.getMessage(); 
}
```
Instead use the exception:
```sh
try {
// statements
} catch (SomeException se) { 
ApexPages.addMessages(se); 
}
``` 
