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

