# Visualforce Best Practices 
Salesforce provides good guidelines for improving visualforce pages performance and design considerations. Refer below links for detailed documentation

http://wiki.developerforce.com/page/Visualforce_Performance:_Best_practices

http://www.salesforce.com/us/developer/docs/pages/index.htm

These documentation covers best practices for
-	Improving Visualforce Performance
-	Accessing Component IDs
-	Static Resources
-	Controllers and Controller Extensions
-	Using Component Facets
-	Page Block Components
-	Rendering PDFs
-	<apex:panelbar>

Some general considerations are 
-	Follow general Web design best practices, such as the minification of JavaScript and CSS, optimizing images for the Web, and avoiding iframes whenever possible.
-	View state size should be kept minimum as possible
-	Cache any data that is frequently accessed, such as icon graphics
-	Avoid SOQL queries in your Apex controller getter methods
-	Improve Visualforce page load times
-	Prevent Field Values from Dropping off the Page
-	By using the with sharing keyword when creating your Apex controllers, you have the possibility of improving your SOQL queries by only viewing a data set for a single user.
-	Move non-essential logic to an asynchronous code block using Ajax.
-	Multiple concurrent request should be optimised.
