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
17|	"Trigger: Trigger class header comment should be in below format: <br/>/** <br/>* Author: cognizant Team<br/>  * Description: <Trigger Description><br/>  * Date Created: <Date the Trigger was created> <br/>  * Version: <version Number> Note – It should start from 0.1 and increment every time we are<br/>   *    making changes to the class<br/>  */ "	

# VF Pages
|#|Code Review Condition|Validated (Y/N)|Reviewer Comments|	Remarks|
|--|--|--|--|---|
1|	VF Pages: A page name can consist of alphanumeric characters. It should be unique, beginning with an uppercase letter. It should not contain underscores and spaces. The words should be concatenated with Initial uppercase. Eg. ConfigureOpportunity.			
2|	VF Pages: Allignments should be properly maintained with a tab indent of four spaces.			
3|	VF Pages: Define a String constant (static final) for messages/text that are not within the System.Label collection and that are not defined as a Custom Setting. 			
4|	"VF Pages: Every visual force page must have an <apex:messages /> tag for the display of custom validation and other error messages.<br/><br/>Example of Visualforce Page Using apex:messages<br/><br/><apex:page controller=""AccountWizardController"" > <br/> <apex:form > <br/>  <apex:pageBlock mode=""detail""><br/>   <apex:messages /><br/> <<apex:pageBlockButtons>><br/>   <apex:commandButton action=""{!sa}"" Value=""Sa""/><br/>  </apex:pageBlockButtons><br/>  </apex:pageBlock><br/>    </apex:form ><br/></apex:page>"			
5|	VF Pages: Give a meaningful description for each Visualforce page		
6|	VF Pages: In “simple links” (non-actions), you must use apex:outputLink(non-action) instead of apex:commandLink(action)
7|	VF Pages: In actions, when applicable, you must use the rerender attribute to refresh a part of page instead of submitting whole Visualforce page.
8|	VF Pages: No duplicate labels present.
9|	VF Pages: Place images, and other “static” resources for use with a Visualforce page, into a static resource. Then, from the Visualforce page, reference that static resource.
10|	VF Pages: Provide meaningful comments before every <apex:page> element.
11|	VF Pages: Should start the custom label name with uppercase letter and it should be short and meaningful. Words in the name should be separated by underscore.
12|	VF Pages: Use global variables to reference general information about the current user and your organization on a Visualforce page. All global variables must be included in expression syntax, for example, {!$User.Name}.
13|	VF Pages: Use the System.Label collection to access/use labels, that have been defined. All error messages, etc.must be defined as either a system label or a custom label.
14	|VF Pages: Visualforce tag should have an ID and that ID should be unique.
15|	VF Pages: You must define and use custom labels for all other UI labels, prompts, button labels, error messages, etc., when the label you want does not exist within $Label.Site. Custom labels are available in the $Label global variable.
16|	VF Pages: You must mark a controller variable as “transient” if the variable is not needed between server calls.
17	|"VF Pages: You must use the standard field labels for all UI labels. For example, <br/>To use the standard field name for the Name field, in the Account object, instead use<br/>{!$ObjectType.Account.Fields.Name.Label}.<br/>To use the standard plural field name for the Name field, in the Account object, use <br/>{!$ObjectType.Account.Fields.Name.LabelPlural}."
18|	VF Pages: You must use Visualforce standard functions and components wherever available instead of creating different/separate functions and components.
19|	VF Pages: Has the unnecessary/commented code removed?
20|	VF: Page redirection - Are there any hardcoded URLS? Is Page.getURL() being used?
21|	"VF Pages: A cross-site scripting weakness occurs when dynamically generated web pages display unvalidated, unfiltered, and unencoded user input allowing an attacker to embed malicious scripts into the generated page.<br/>Need to encode the parameters so that attackers cannot hack, listed below are the encoding functions:<br/>1. HTMLENCODE<br/>2. JSENCODE<br/>3. URLENCODE<br/>4. JSINHTMLENCODE<br/><br/>EXample: Incorrect script tag - <script>var foo ='{!$CurrentPage.parameters.userparam}';</script><br/>Correct script tag - <script>var foo = '{!JSENCODE($CurrentPage.parameters.userparam)}';</script>"
22|	"VF Pages: VF Page Header example:<br/> <!-- <br/> Author: <br/>  Date Created  : <br/>  Description     : <br/>-->"
23|	VF Pages: Do not use &nbsp, <br> tags, instead implement using CSS. 
24|	VF Pages: You must use the standard labels available in the $Label.Site global variable, for all user-interface (UI) labels, prompts, button labels, error messages, etc., when the label you want exists within $Label.Site. As a corollary, this mean that you must not create a custom label if an appropriate standard label already exists.
25|	"VF Pages: You must use the standard field labels for all UI labels. For example, to use the standard field name for the Name field, in the Account object, instead use<br/> {!$ObjectType.Account.Fields.Name.Label}.<br/>To use the standard plural field name for the Name field, in the Account object, use {!$ObjectType.Account.Fields.Name.LabelPlural}."
26	|"Inline Comments for Pages:<br/><!-- Your Comments --> Note – Mention Release Specific details in Inline comment wherever required"

# CSS & JavaScript
|#|Code Review Condition|Validated (Y/N)|Reviewer Comments|	Remarks|
|--|--|--|--|---|
1|	"CSS: You must place all CSS within a separate stylesheet, as a static resource. You must not place any CSS styles within a Visualforce page.<br/><br/>Example of Including Static CSS Resource<br/><apex:stylesheet value=""{!$Resource.MyStylesheet}""/>"			
2|	CSS: You must place the <apex:stylesheet> tag at the top of the Visualforce page, before any content that will use any styles within the stylesheet. 			
3|	"Javascript: If there is a JavaScript with in a Visualforce page, put into a separate JavaScript file, as a static resource. Then from the Visualforce page, call the JavaScript .<br/><br/>Example of Including Static JavaScript Resource<br/><apex:includeScript value=""{!$Resource.MyJavascriptFile}""/>"			
4|	Javascript: Is the <apex:includeScript> tag at the bottom of the Visualforce page, just before the </apex:page>.			
5|	Javascript: You must use the $Component global variable to retrieve a Visualforce component’s DOM ID.		
# Test Class
|#|Code Review Condition|Validated (Y/N)|Reviewer Comments|	Remarks|
|--|--|--|--|---|
1|	Test Class: A separate test class should be there for each Apex class.
2|	Test Class: All the test data creation should be before Test.startTest(). 
3|	Test Class: Assert statements for async call like batch classes, scheduler, future methods should be after the Test.stopTest().
4|	Test Class: For conditional logic (including ternary operators), execute each branch of code logic.
5|	Test Class: Must have at least 95% of your Apex code covered by unit tests to deploy your Apex code to production environments. Comments and System.debug() calls are not included in the Code coverage.
6|	Test class: Test Apex class methods and Trigger using both valid and invalid inputs 
7|	Test Class: Test classes follow naming convention <ClassName>_Test.
8	|Test Class: Test classes should  cover bulk scenarios for all helper classes that are called from any trigger.
9|	Test Class: Trigger test classes should  test the trigger’s bulk handling capability, using at least 200 records. 
10|	Test Class: Use at least one System.assert(), System.assertEquals(), or System.assertNotEquals() within every test method. Make sure specific asserts been used in the program.
11|	Test Class: Use the System.RunAs() method to test your application in different user contexts. Make sure to test with the right user apart from system admin.
12	|Test Class: Write comments stating not only what is supposed to be tested, but also the assumptions the tester made about the data, the expected outcome, and so on.
13	|Test Class: Has the unnecessary/commented code removed?
14|	Test Class: Do the test classes use the utility methods to insert data on the fly including the Master table data. ?
15|	"Test Class: You must provide a class header, similar to the following, at the start of every class. The header must use the Javadoc syntax.<br/>Example of Class Header<br/>/**<br/> @Author <Author Name><br/>@name <Class name><br/>@CreateDate <Date><br/>@Description <purpose of the class><br/> @Version <1.0><br/>@reference <Referenced program names><br/>*/"
16|	"Test Class: You must provide a method header, similar to the following, at the start of every public and protected method. <br/><br/>Example of Method Header<br/>/**<br/>*  Description of the purpose of the classthe method. <br/>*  @name <method-name><br/>*  @param <parameter-name> <description><br/>*  @return <parameter> - <Description about the return parameter><br/>*  @throws exception-<exception description><br/>*  @see com.ac.sample.SuperObject#get<br/>*/"
17|	Test Class: No hard coding present in test class except which are been used only within the test class and not with Org.
18|	Test Class: Test class access specifier should be private and not public.
19	|Test Class: Test class should have postive and negative scenarios should capture exact negative scenario and not generic.
20|	Test Class: No need to insert the user, as it is used only for runAs(). Just define the user.
21|	Test Class: Ensure that the user names are uniqe accross the organization accross all the test classes. Provide the user names very unique and specific to the test class instead of generic names.
22|	Test Class: Multiple DML statements should be avoided instead use collection and use one DML statement.
23|	Test Class: Batch classes should test at least for 200 records in the test class itself.
24|	Test Class: Test Class required for configuration components namely Validation rules and Workflow rules.

# Error Handling
|#|Code Review Condition|Validated (Y/N)|Reviewer Comments|	Remarks|
|--|--|--|--|---|
1|	Error Handling Info: “unhandled” exceptions, thrown within the SFDC platform must use the custom Apex logging class to log the exception.
2|	Error Handling: Apex controllers for Visualforce pages, handle all possible exceptions and return it to the current page reference via ApexPages.addMessage() method.
3|	"Error Handling: Code like this may cause the error log to get rolled back if the exception goes unhandled (applicable for VisualForce Controller or Apex Class).  Avoid this:<br/> <br/>try {<br/>// Perform some function<br/>} catch (Exception e) {<br/>LoggerService.logHandledException(e, …);<br/>throw e;<br/>}"
4	|"Error Handling: Instead, error logging should happen at the outer most layer (Entry Method) possible (applicable for VisualForce Controller or Apex Class)<br/> <br/>Savepoint outerSavepoint = Database.setSavepoint();<br/>try {<br/>// Perform some function<br/>} catch (Exception e) {<br/>Database.rollback(outerSavepoint);<br/>LoggerService.logHandledException(e, …);  <br/>// Notify user of error here<br/>}"
5|	Error Handling: If an exception is handled in a try-catch block, the exception information could be stored in a custom object and a meaningful message should be displayed on the UI. (Applicable for VF Page)
6|	Error Handling: If exception is caught in a place other than a public method that is the entry point for a service call, then a more specific type of exception should be caught.
7|	Error Handling: No empty catch blocks, if you need to catch an exception & not do anything after catching it, there must be a comment explaining why nothing is being done.
8|	"Error Handling: No Nonpublic Personal Information (NPI) data is logged or included as part of any exception thrown across layers within the architecture.<br/><br/>Example of Including NPI Data (DO NOT USE THIS CODE)<br/>AnExceptionClass e = new AnExceptionClass('Error occurred on account with an SSN of ' + someSSN + '.');"
9|	Error Handling: Provide exception handling (try-catch block) to handle direct DML (excluding database methods) exceptions.
10|	Error Handling: Are null pointer checks (Isempty, Size()>0, Isnull, Isnotnull etc) made wherever necessary?
11|	Custom code calling web services should catch SOAP faults and log exceptions using LoggingService.logServiceException
# General & Security & Governor
|#|Code Review Condition|Validated (Y/N)|Reviewer Comments|	Remarks|
|--|--|--|--|---|
1|	General: Identify if anything can be put into a separate method so that the readability and useability are taken care.			
2|	General: No single-letter variable names other than loop control variables to be used.			
3|	General: Split lines to avoid excessively long lines.			
4|	General: Use Database.insert() whenever feasible.			
5|	General: You must remove all commented-out code before checking-in code.			<br/>	General: System.Debug statements should be removed.			
6|	Governer Limits: Do not call any asynchronous (@future) method inside any loop.			
7|	Governer Limits: Do not execute any query or Data Manipulation Language(DML) statements in a loop, use a SOQL for loop.			
8|	Governer Limits: Do not execute Data Manipulation Language(DML) or SOQL at class initialization (in constructor).			
9|	Governer Limits: Reduce the number of DML operations by adding records to collections and performing DML operations against these collections.			
10|	Governer Limits: Reduce the number of SOQL statements by preprocessing records and generating sets, which can be placed in single SOQL statement used with the IN clause or WHERE clause.			
11|	Governor Limits: If you must use dynamic SOQL, use the escapeSingleQuotes method to sanitize user-supplied input.  Example is \'<field>\'			
12|	Security: Any password and sensitive information stored in a Salesforce object must be encrypted and stored.			
13|	Security: Session ID should always be encrypted in transmission. Session ID should not be sent to third parties. 			
14|	"Security: Validate that the connection is being requested from a valid Salesforce server. Below is the regex to validate legitimate SFDC SOAP servers: https://[^/?]+\\.(sales\|visual\\.)force\\.com/services/(S\|\s)(O\|o)(A\|a)(P\|p)/(u\|c)/.*"			
15|	Limit line length to 125 characters.			




