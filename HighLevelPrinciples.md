# High Level Principles

- Code compiles with no errors or warnings. 
-   All code is committed to approve source control. Commits are atomic and frequent, are accompanied by a short description of the change, and are pushed to the remote repository at least daily. 
-   Naming Conventions are adhered to. 
-   Never store a username and/or password in code or source control. 
-   Never hard coded Salesforce IDs or URLs or profile/permission names in code. 
-   All code must have 90% or above code coverage using an approved test framework. -   Best practice for this is 95% but this has been slightly relaxed on case to case basis with agreement with customer IT team. 
-   Always use the latest generally available version of the Salesforce API. 
-   Follow the code commenting guidelines. 
-   Code must provide proper Error and Execution Handling as advised below
-   Security audits must be performed with appropriate tools ( CheckMarx or Burp Suite ).
-   All triggers should be bulkified. 

The Platform Owner retains ultimate control of the standards, and may overrule this document on request in specific cases. This can be requested by projects through Customer Salesforce IT team for the Salesforce platform.

See more on [General Naming Convention.md](https://github.com/joemccann/dillinger/blob/master/KUBERNETES.md)