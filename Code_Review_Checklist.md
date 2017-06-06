# Apex Classes
|#|Code Review Condition|Validated (Y/N)|	Reviewer Comments|	Remarks|
|-|----|---|---|--|
|1|Apex Class: All constants should have uppercase names, with logical sections of the name separated by underscores.||
|2|"Apex Class: Check return value from all Database.delete(), Database.emptyRecycleBin(),Database.insert(),Database.undelete(), Database.update(), and Database.upsert() calls.<br/>If a list passed to any of these methods the call will return an array of results;code must check the all the return values in the returned array of results.<br/>  <br/>updateMsg=Database.update(messageHeaderObj,false);<br/>if (!updateMsg.isSuccess()) { Database.Error[] err = updateMsg.getErrors();<br/>faultLogList.add(UTIL_MarketingManagement.getFaultLogWithStaticValues(UTIL_Constants.TRANSACTION_TYPE_MESSAGE,<br/>UTIL_Constants.CLASS_NAME, <br/>err[0].getStatusCode().name()+''+updateMsg.Id, <br/>updateMsg.getErrors()[0].getMessage()));<br/>}"
3	|Apex Class: Classes that contain methods or variables defined with the webService keyword must be declared as global.			
4|	Apex Class: Declare instance variables as private or protected.			
5|	Apex Class: Declare only one variable per line of code.			
6|Apex Class: Define meaningful numbers using clearly named constants (static final).			
7|Apex Class: Do not instantiate any object before you actually need it. Specifically, do not instantiate objects in variable declarations outside of a try-catch block.			
8|Apex Class: Each method name should have meaningful names starting with lowercase and following words should have initial uppercases.			
9|Apex Class: For each database applicable operation (like Database.update(), Database.insert(), etc., and DML statements), carefully consider what value to use for opt_allOrNone (for Database) and the optAllOrNone property (for DML)			
10	|"Apex Class: Getter and setter methods of class variables use the naming convention of ""get""/""set"" followed by the variable name, with the first letter of the name capitalized.<br/> <br/> Example of a Getter and Setterpublic string customerID;    // Variable declaration section<br/> public string getCustomerID() {  //Get method definition<br/> return <string>;<br/> }<br/> public string setCustomerID() {  //set method definition<br/> return <string>;<br/> }<br/> Apex class properties may be used instead of getter and setter methods."		
11|	"Apex Class: You must handle type IDs, record type IDs, etc. via lookup (i.e., you must not hard code record type IDs).<br/> <br/> Example of Hard-coded Record Type IDs (do NOT do this)<br/> <br/> For (Account a :Trigger.new){<br/> //Problem - hardcoded the record type id<br/> if (a.RecordTypeId=='012500000009WAr') {<br/> //  do some logic here...<br/> } else if (a.RecordTypeId=='0123000000095Km') { <br/> // do some logic here for a different record type <br/> }}<br/> <br/> To properly handle the dynamic nature of the record type IDs, the following example queries for the record types in the code, stores the dataset in a map collection for easy retrieval, and ultimately avoids any hardcoding.<br/><br/>  Example of Non-hard-coded Record Type IDs (DO this)<br/> <br/> //Query for the Account record types<br/> List<RecordType> rtypes = [Select Name, Id From RecordType<br/> where sObjectType='Account' and isActive=true];<br/> // Create a map between the Record Type Name and Id for easy retrieval<br/> Map<String,String> accountRecordTypes = new Map<String,String>{};<br/> for (RecordType rt : rtypes){<br/> accountRecordTypes.put(rt.Name,rt.Id);<br/> }<br/> for (Account a: Trigger.new) {<br/> // Use the Map collection to dynamically retrieve the Record Type Id<br/> // Avoid hardcoding Ids in the Apex code<br/> if (a.RecordTypeId == accountRecordTypes.get('Healthcare')){<br/> //do some logic here...<br/> } else if (a.RecordTypeId == accountRecordTypes.get('High Tech')) {<br/> // Do some logic here for a different record type...<br/> }<br/> }"			
12|	Apex Class: If a method, variable or inner class is declared as global, the outer, top-level class must also be defined as global			
13|	Apex Class: Initialize all Boolean, Integer, Decimal, Double, and Long variables (instance, class, and method scope) at the point of declaration, to a non-null value. 			
14	|Apex Class: Methods must have no more than two levels of looping or other control structures.			
15|	Apex Class: Methods should have a try-catch-finally block to handle possible exceptions. 			
16|	Apex Class: Never hardcode an id, link, etc. in code; instead, you should use Custom Labels or static Apex classes with constants (public static final) or enums.			
17|	Apex Class: Precede each IsEmpty() method call with a null check.			
18|	Apex Class: Should initialize all variables before using them. Apex initializes all variables to null by default.			
19|	Apex Class: To convert a non-String variable to a String, use String.valueOf(varName) instead of varName + "".			
20	|Apex Class: Use sets, maps, or lists when returning data from the database. This makes your code more efficient because the code makes fewer trips to the database.			
21	|Apex Class: Use the Page.getUrl() method to get the URL/URI for a Salesforce page.			
22|	Apex Class: When comparing a String literal to a String object, always use the syntax 'String literal'.equals(stringObj).			
23|	Apex Class: When writing a VF Controller, make sure to add 'with sharing' in the class declaration. This will help in controlling the code coverage not going down for different user profiles.			
24|	"Apex Class: You must provide a class header, similar to the following, at the start of every class.<br/>  The header must use the Javadoc syntax.<br/> <br/> Example of Class Header<br/> /* <br/> @Author <Author Name><br/> @name <Class name><br/>  @CreateDate <Date><br/> @Description <purpose of the class<br/>  @Version <1.0><br/> @reference<Referenced program names><br/>  */"			
25|	"Apex Class: You must provide a method header, similar to the following, at the start of every public and protected method.<br/> <br/>  Example of Method Header<br/> /**<br/> *  Description of the purpose of the classthe method. <br/>  *  @name <method-name><br/> *  @param <parameter-name> <description><br/> *  @return <parameter> - <Description about the return parameter><br/> *  @throws exception-<exception description><br/> */"			
26|	Are System.debug statements removed?			
27|	Apex Class: Has the unnecessary/commented code removed?			
28|	Apex Class: No switch/case statements are allowed. This also goes for if/then/else statements that effectively do the same thing as a switch statement.			
29|	Apex Class: Are SOQL queries written outside the For loop? Make sure that methods within For loops do not have SOQL queries till inner methods.			
30|	Apex Class: Check all queries used in code are strictly necessary and no further Optimization can be done.			
31|	Apex Class: Has all repetitive code moved to a method? 			
32|	Apex Class: Instead of storing a query in a list and iterating through that List in for loop, Query directly inside the condition of for each Loop, so it iterates directly over query.			
33|	Apex Class: Combine If statements wherever possible.			
34|	Apex Class: Executable statement count cannot exceed 30 for each method (Braces ,comments, and blank lines are not included in the limit of 30.)			
35	|Apex Class: Are there unnecessary Null Checks?			
36|	Apex Class: Is proper access modifier used for methods and variables ?			
37|	Apex Class: Same Method Should Not Be Used in Main Class and Test Class			
38|	Apex Class: Limit Length of WHERE Clause and no query without WHERE clause.			
39	|Apex Class: Don’t send parameters  to a method if they can be created within that method			
40|	Apex Class: Reduce number of statements by combining multiple statements  where possible			
41|	Apex Class: Using keyset to iterate			
42|	"Apex Class: SOQL Injection may cause, wrong approach is as below:<br/> public PageReference query() {<br/> String qryString = 'SELECT Id FROM Contact WHERE ' + '(IsDeleted = false and Name like \'%' + name + '%\')';<br/> queryResult = Database.query(qryString) ;<br/> return null;<br/> }<br/> <br/> Right approach is as below:<br/> publicPageReference query() {<br/> String queryName = '%' + name + '%'queryResult = [SELECT Id FROM Contact WHERE (IsDeleted = false and Name like :queryName)];<br/> return null;<br/> }"			
43|	Apex Class: Is there any empty Constructor in the class?			
44|	Apex Class: For a List, is the isEmpty() check present instead of size()? This will improve the performance.			
45|	Apex Class: Is there different method returning the same value?			
46|	Apex Class: Use ".equalsIgnoreCase' instead of "==" when comparing strings.			
47|	Apex Class: In If statements, when comparing against a constant, put it first so that it is impossible for a null exception to occur.			
48	|"Apex Class: Use isNotBlank() method for checking strings, rather than Null and ' '.Use isBlank method on the string instead of comparing with null and empty string. isBlank method check for both the options."			
49|	Apex Class: No need to check for "== true" for boolean variables.			
50|	Apex Class: Instead of specifying all the fields in the query, use schema methods to iterate and build query string. In future even there is new field is added the code will be handled automatically.			
51|	"Apex Class: for(MessageHeader__c messageObj:messageSch)<br/> { listIdSet.add(messageObj.List__c);<br/> }<br/> No need of 'for loop' here to convert list to set. Use Set.addAll(List) method."			
52|	Limit line length to 125 characters.			
53|	"Inline Comments for Class and Methods:<br/> // Your Comments Note – For comments regarding a particular line; Mention Release Specific details in Inline comment wherever required<br/> OR<br/> /* Your Comments*/ Note – For comments related to a block; Mention Release Specific details in Inline comment wherever required"	


# Trigger
|#|Code Review Condition|Validated (Y/N)|Reviewer Comments|	Remarks|
|--|--|--|--|---|
1|	Trigger Info: Triggers can be used to prevent DML operations from succeeding by calling the addError() method on a record or field. When used on Trigger.new records in insert and update triggers, and on Trigger.old records in delete triggers, the custom error message is displayed in the application interface and logged.			
2|	Trigger: Minimize the amount of code within a trigger. To simplify testing and reuse, triggers should delegate to Apex classes that contain the actual execution logic. 			
3|	Trigger: Prevent an infinite loop of execution of a trigger while creating triggers on parent and child objects, or updating same objects from the after update triggers. 			
4|	Trigger: Provide a maximum of one trigger per event per object.			
5|	Trigger: Provide bypass logic in each trigger. The bypass logic provides an easy way to bypass (temporarily disable) the trigger.			
6|	Trigger: Set only a maximum of five save points in all contexts			
7|	Trigger: Use static variables to pass data between triggers. 			
8|	Trigger: User-defined methods in a trigger cannot be declared as static			
9|	Trigger: When delegating from a trigger to a helper Apex class, pass only the appropriate records to the helper class for processing. 			
10|	Trigger: Has the unnecessary/commented code removed?			
11|	Trigger: Has the trigger logic been written in an Apex class and not within the trigger?			
12|	Trigger: Triggers Should Include Try/Catches			
13|	Trigger: Implement Check Limits in all trigger methods			
14|	Trigger: Implement SaveResults checks in all trigger methods			
15|	Trigger: Handling of Errors in "after" trigger			
16|	Trigger:You must write triggers to support bulk operations (i.e., you must bulkify triggers) of up to 200 records for each call.			
17|	"Trigger: Trigger class header comment should be in below format: <br/>/** 
   <br/>* Author: cognizant Team
 <br/>  * Description: <Trigger Description>
 <br/>  * Date Created: <Date the Trigger was created> 
 <br/>  * Version: <version Number> Note – It should start from 0.1 and increment every time we are 
<br/>   *    making changes to the class
 <br/>  */
           "			



