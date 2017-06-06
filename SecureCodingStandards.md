# Secure Coding Standards
## Security and the Force.com Platform
- Force.com was designed to be flexible support developer and  business needs
- Force.com provides many built-in protections to protect developers  and their user base
- Salesforce protects end users by ensuring that all applications  listed in the AppExchange undergo a security review
## What kinds of vulnerabilities do we typically  see?
- Cross-Site Scripting (XSS)
- CRUD/FLS Enforcement
- Information Disclosure
- Sharing Violation
- Cross-Site Request Forgery
- Insecure Storage of Sensitive Data
- Authentication/Authorization
- SOQL/SQL Injection
- Insecure Session Cookie Handling

## Cross-Site Scripting (XSS)
- XSS (Cross-Site Scripting) attacks occur when malicious code is  sent via a web application if this does not validate/encode the user  input
- In the simplest form, a web app is said to be subject to XSS attacks  when user supplied input is directly reflected back without encoding

## Types of XSS attacks
- Reflected XSS: malicious input is reflected back without proper  encoding
- Stored XSS: vulnerable server permanently stores the user’s  malicious input
- DOM XSS attacks: attack payload gets executed and modifies the  DOM in the victim’s browser

## Force.com XSS Protections
- Key defense mechanisms: input filtering and output encoding on  user supplied input
- To encode user input you may use <apex:outputText>,  HTMLENCODE(), JSENCODE(), or JSINHTMLENCODE().

## Force.com XSS Protections
- <apex:outputText>
Hello {!$CurrentPage.parameters.userName}
</apex:outputText>
- var a = '{!JSENCODE($CurrentPage.parameters.userName)}';
- <apex:column id="message" value="{!message}"  onclick="displayPopup('{!JSINHTMLENCODE(message))}')"/>
- HTML encode in Apex with escapeHtml3()/escapeHtml4()

## Testing for XSS
- Blackbox testing
> Inspect page source to check if user input is encoded
- Whitebox testing
> Look for anti-­‐patterns, e.g. escape=”false”, innerHTML assignments, ...

# SOQL Injection
## SQL injection
- SQL Injection is an attack where user input is allowed to modify the  structure of an SQL query and perform unexpected actions
- Sample SQL query subject to SQL injection:
```sh
SELECT *
FROM USER
WHERE username = 'un_input'
AND password = 'pw_input'
```
- If un_input = “admin--” and user input is not modified before passing it to  the query we get:
```sh
SELECT *
FROM USER
WHERE username = 'admin--' and password = 'pw_input'
```
## SOQL vs SQL
- SOQL is the query language used in the Salesforce platform
- SOQL only allows the SELECT command portion of SQL
- SOQL does not allow command execution, or wild card (*) for  fields.

## SOQL injection
- SOQL injection only occurs when dynamic SOQL queries are used without  proper manipulation of user input
- Sample code block:
```sh
String query = 'select id from account where name =\'';
query += userinput + '\''
queryResult = Database.query(query);
```
- User input:
```sh
Bob' OR AnnualRevenue > 1000000 OR name ='BOB
```
- Final Query:
```sh
SELECT id
FROM account
WHERE name = 'Bob'
OR AnnualRevenue > 1000000
OR name ='Bob'
```
## SOQL injection mitigations
- Static query + bind variable:
```sh
queryResult = [select id from account where name = :var]
```
- Wrap user input in string.escapeSingleQuotes()
- This will not prevent all the attacks.
- Sample query:
```sh
String query = 'select id from user where isActive =' +
string.escapesinglequotes(var);
-  User input that could bypass this defense mechanism:
true AND RecievesAdminInfoEmails = true
```
## SOQL mitigations
- Replace unexpected characters:
```sh
String query = 'select id from user where isActive ='
    + var.replaceAll('[^\\w]','');
```
- Proper typecasting:
```sh 
public Id x {get; set;}
String query = 'select id from Account where ID = '
    +string.valueOf(x);
```

## Summary
- Always assume any user controllable input has been tainted
- Leverage Force.com built-in protections where appropriate
- Perform regular peer reviews of code

## References
- http://sfdc.co/CodingGuidelines
- http://sfdc.co/XSS
- http://sfdc.co/SOQLInjection
- http://sfdc.co/OpenRedirect

## Salesforce.com Don’ts :
- Hard coding Country based business logic in code
- DML Statements and Apex Callouts are not embedded within an Exception handling  statement
- Hard coded Salesforce URLs
- Avoid window.location.replace in lightning Components
- Result of a query might have more than one record but assigned to single sObject
- Hard-coded Ids
- Declaring class without sharing keywords
- Calling SOQL and DML inside loop
- Writing SOQL without Where or Limit Clause
- Writing test class without Test.Start () and Test.Stop () 
- Writing Apex / Test class without bulkifying
- Multiple triggers against single objects 
- Usage of SeeAllData=True annotation in Test classes 