# Code security and Secure Coding 

Salesforce has incorporated several security defenses into the Force.com platform itself.

However, careless developers can still bypass the built-in defenses in many cases and expose their applications and customers to security risks. Many of the coding mistakes a developer can make on the Force.com platform are similar to general Web application security vulnerabilities, while others are unique to Apex.

Security audits for Force.com code should be done with the Checkmarx tool at http://security.force.com/. Running Checkmarx regularly should be an integral part of every sprint to identify issues. Sonar cube

### Standard Visualforce components

All standard Visualforce components, which start with <apex>, have anti-XSS filters in place. For example, the following code is normally vulnerable to an XSS attack because it takes user-supplied input and outputs it directly back to the user, but the <apex:outputText> tag is XSS-sSFAe. All characters that appear to be HTML tags are converted to their literal form. For example, the < character is converted to < so that a literal < displays on the user's screen.
```
<apex:outputText> {!$CurrentPage.parameters.userInput}
</apex:outputText>
```
By default, virtually all Visualforce tags escape the XSS-vulnerable characters. It is possible to disable this behavior by setting the optional attribute escape="false". This default behavior must not be disabled without CTO and Security Approval.

### Programming Items Not Protected from XSS

The following items do not have built-in XSS protections, so take extra care when using these tags and objects. This is because these items were intended to allow the developer to customize the page by inserting script commands. It does not makes sense to include anti-

XSS filters on commands that are intentionally added to a page.

##### Custom JavaScript

If you write your own JavaScript, the Force.com platform has no way to protect you. For example, the following code is vulnerable to XSS if used in JavaScript. Care should be taken when using JavaScript, and code must be subject to rigorous VA where such methods are used.
```javascript
var	foo = location.search; 
document.write(foo);
```

#### <apex:includeScript>
The <apex:includeScript> Visualforce component allows you to include a custom script on the page. In these cases, ensure the content is sSFAe and does not include user-supplied data. For example, the following snippet is extremely vulnerable because it includes user-supplied input as the value of the script text. The value provided by the tag is a URL to the JavaScript to include. If an attacker can supply arbitrary data to this parameter, they can potentially direct the victim to include any JavaScript file from any other website.
```
<apex:includeScript value="{!$CurrentPage.parameters.userInput}" />
```

### Formula Tags

The general syntax of these tags is:{!FUNCTION()} or {!$OBJECT.ATTRIBUTE}.

Formula expressions can be function calls or include information about platform objects, a user's environment, system environment, and the request environment. An important feature of these expressions is that data is not escaped during rendering. Since expressions are rendered on the server, it is not possible to escape rendered data on the client using JavaScript or other client-side technology. This can lead to potentially dangerous situations if the formula expression references non-system data (that is potentially hostile or editable data) and the expression itself is not wrapped in a function to escape the output during rendering.

The standard mechanism to do server-side escaping is through the use of the

SUBSTITUTE() formula tag.

Formula tags can also be used to include platform object data. Although the data is taken directly from the user's organization, it must still be escaped before use to prevent users from executing code in the context of other users (potentially those with higher privilege levels). While these types of attacks must be performed by users within the same organization, they undermine the organization's user roles and reduce the integrity of auditing records. Additionally, many organizations contain data which has been imported from external sources and might not have been screened for malicious content.

### Cross-Site Request Forgery (CSRF)

Cross-Site Request Forgery (CSRF) flaws are less of a programming mistake as they are a lack of a defense. The easiest way to describe CSRF is to provide a very simple example.

An attacker has a Web page at www.attacker.com. This could be any Web page, including one that provides valuable services or information that drives trSFAfic to that site.

If the user is still logged into your Web page when they visit the attacker's Web page, the URL is retrieved and the actions performed. This attack succeeds because the user is still authenticated to your Web page.

Within the Force.com platform, Salesforce has implemented an anti-CSRF token to prevent this attack. Every page includes a random string of characters as a hidden form field. Upon the next page load, the application checks the validity of this string of characters and does not execute the command unless the value matches the expected value. This feature protects you when using all of the standard controllers and methods.

Developers must not bypass these built-in defenses, either deliberately or inadvertently. Developers should be cautious about writing pages that take action based upon a user-supplied parameter.

### SOQL Injection

In other programming languages, the previous flaw is known as SQL injection. Apex does not use SQL, but uses its own database query language, SOQL. SOQL is much simpler and more limited in functionality than SQL. Therefore, the risks are much lower for SOQL injection than for SQL injection, but the attacks are nearly identical to traditional SQL injection.

In summary SQL/SOQL injection involves taking user-supplied input and using those values in a dynamic SOQL query. If the input is not validated, it can include SOQL commands that effectively modify the SOQL statement and trick the application into performing unintended commands.

To prevent a SOQL injection attack, dynamic SOQL queries should not be used. Instead, use static queries and binding variables.

If you must use dynamic SOQL, you must use the escapeSingleQuotes method to sanitize user-supplied input. This method adds the escape character (\) to all single quotation marks in a string that is passed in from a user. The method ensures that all single quotation marks are treated as enclosing strings, instead of database commands.

### Data Access Control
The Force.com platform makes extensive use of data sharing rules. Each object has permissions and may have sharing settings for which users can read, create, edit, and delete. These settings are enforced when using all standard controllers.

When using an Apex class, the built-in user permissions and field-level security restrictions are not respected during execution. The default behavior is that an Apex class has the ability to read and update all data within the organization. Because these rules are not enforced, developers who use Apex must take care that they do not inadvertently expose sensitive data that would normally be hidden from users by user permissions, field-level security, or organization-wide defaults. This is particularly true for Visualforce pages.
```
public class customController 
{ 
public void read() 
{
Contact contact = [SELECT id FROM Contact WHERE Name = :value];
}
}
```
In this case, all contact records are searched, even if the user currently logged in would not normally have permission to view these records. The solution must use the qualifying keywords with sharing when declaring the class:
```
public with sharing class customController {
...
}
```
The with sharing keyword directs the platform to use the security sharing permissions of the user currently logged in, rather than granting full access to all records.

Alternatively, you may use the isAccessible, isCreateable, or isUpdateable methods of Schema.DescribeSObjectResult to verify whether the current user has read, create, or update access to an sObject, respectively. Similarly,Schema.DescribeFieldResult exposes these access control methods that you can call to check the current user's read, create, or update access for a field. In addition, you can call the isDeletable method provided by

Schema.DescribeSObjectResult to check if the current user has permission to delete a specific sObject.

```
if	(Schema.sObjectType.Contact.fields.Email.isUpdateable()) { // Update contact phone number
}
```





