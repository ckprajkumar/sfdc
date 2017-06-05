# 6.3.	Unit Test Coverage
To facilitate the development of robust, error-free code, Apex supports the creation and execution of unit tests. Unit tests are class methods that verify whether a particular piece of code is working properly. Unit test methods take no arguments, commit no data to the database, send no emails, and are flagged with either the @isTest annotation on the class, or the testMethod keyword in the method.

Either:
```sh
@isTest

Public class myClass_Test{ // Methods for testing

@isTest static void test1() { // Implement test code
}
}
```
or
```sh
public class myClass_Test {

static testMethod void test1 () { // Implement test code
}
}
```
The following need to be adhered to when writing test code
-	Cover as many lines of code as possible 
-	You must have 90% of your Apex scripts covered by unit tests to deploy your scripts to pre prod and production environments. In addition, all triggers must have unit test coverage. 
-	Calls to System.debug are not counted as part of Apex code coverage in unit tests. 
-	In the case of conditional logic (including ternary operators), execute each branch of code logic. 
-	Make calls to methods using both valid and invalid inputs. 
-	Complete successfully without throwing any exceptions, unless those errors are expected…catch andblockcaught. in a try 
-	Always handle all exceptions that are caught, instead of merely catching the exceptions. 
-	Use System.assert methods to prove that code behaves properly. 
-	Use the runAs method to test your application in different user contexts. 
-	Use the isTest annotation. Classes defined with the isTest annotation do not count against your organization limit of 1 MB for all Apex scripts. 
-	Exercise bulk trigger functionality—use at least 20 records in your tests. 
-	Write comments stating not only what is supposed to be tested, but the assumptions the tester made about the data, the expected outcome, and so on. 
-	Test the classes in your application individually. Never test your entire application in a single test. 
-	Debug statements and Comments are not included in the Code coverage. 
-	User records are not covered by test methods 
-	Test methods and business methods must not be included inside the same class 
-	There must always be separate methods for parsing the response of a webservice call. This way it will help in writing test methods for code coverage and debugging the code when required. 
-	Avoid using @SeeAllData unless required via the API (eg ConnectAPI) 

## 6.4.	Additional Rules
-	Make sure that each Page items(all VisualForce tags) has ID and the IDs is unique 
-	Pay attention to the comments and remarks and put them in legible form before every Page item 
-	You can use global variables to reference general information about the current user and your organization on a Visualforce page. All global variables must be included in expression syntax, for example, {!$User.Name} 
-	The <apex: includeScript> Visualforce component allows you to include a custom script on the page. In these cases, careful measures need to be taken to validate that the content is sSFAe and does not include user-supplied data. 
-	The alignments have to be properly maintained with a tab of 4 spaces 
-	It is recommended to have partial rendering of a page item then a full page 

## User Interface
By default all pages will use standard Salesforce look and feel. Any customization in this must be discussed with the Design Authority to maintain uniform page layouts in all modules.

If customization of the page is required, then:
-	External CSS must be used - Inline and/or Internal CSS may not be used apart from for base rendering 
-	Modifying the Salesforce CSS is not permitted 
-	For pages that don't use Salesforce CSS files, set the tag’s showHeaders and standardStylesheets attributes to false. This practice excludes the standard Salesforce CSS files from the generated page header. 
-	Review the guidelines from Google PageSpeed Insights (Note that these standards still are dominant where there is a discrepancy) https://developers.google.com/speed/pagespeed/insights/ 

## 6.6.	JavaScript
Use of third party JavaScript frameworks such as jQuery are acceptable however their use should be considered an architecturally significant inclusion for agreement.
-	Load libraries from Salesforce static resources not from external CDNs 
-	Use NoConflict modes as available to avoid overriding $ 
-	Use shared static resources as libraries for repeated code 
-	If possible, load JavaScript libraries at end of page 
-	User-Agent sniffing should be avoided – instead consider using Modernizr for feature detection 
-	Avoid excessive chaining of methods as they make reading code overly complex 
```sh
$($(event.target).parents(‘.wrapper’)[0]).find(‘.record’).find(“.customer id”).innerText().toLowerCase()
```
Avoid inlining javascript – use binding handlers eg
```sh
jQuery.bind();
document.addEventListener()
```

## 6.7.	Triggers
There should only be one trigger per action on an object. The trigger name must be simply the object name and the action. Use trigger handlers to link multiple logical triggers to an event.

To enable co-existance between patterns, we recommend a trigger pattern to support extensions to the existing ITrigger without impacting other projects. This pattern is shown in the appendix.

All triggers must be bulkified and test classes must test the bulkification – regardless of a project’s expectation for bulk invocation.

In addition it must be possible to turn off the complete trigger via a custom setting and static variable.

It must be possible to stop component parts of project functionality in a trigger via a custom setting and static variable.