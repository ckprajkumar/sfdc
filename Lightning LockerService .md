# Securely Using Third-Party Libraries in Lightning Components

- The components you assemble can come from different sources. For example, you can build an application (or a page) by assembling Salesforce components, third-party components, and your own components.
- Components can be built entirely from scratch or by leveraging third-party libraries.

### Which Library Do You Need?
Third-party libraries usually fall in one of the following categories:
- DOM manipulation libraries (jQuery, etc)
- Specialized utility libraries (moment.js, numeral.js, etc)
- UI libraries (Bootstrap, jQuery UI, etc)
- Data visualization libraries (D3, Chart.js, Leaflet, etc)
- MVC frameworks (React, Angular, etc)
#### DOM Manipulation Libraries
        Using jQuery, you would:
            - Assign a unique id to the input element.
            - Use a jQuery selector to find the input element in the DOM.
            - Assign the value to that DOM element.

[Image]

 Using the Lightning Component Framework, you would:
- Declare a score attribute.
- Bind the input field to the score attribute.
- Set the value of the score attribute: The bound input field will automatically display the new value.

[Image]

- When compare to jQuery, lightning component framework is more secured.

#### MVC Frameworks
- At a high level, libraries like React and AngularJS have the same focus as the Lightning Component Framework.
- They provide code organization and utilities to create components. 
- If you are looking for a component framework to develop applications on the Salesforce platform, you should use the Lightning Component Framework because it’s tightly integrated with the platform. 
- But you can also use other frameworks if you so desire. 
- All you have to do is choose the isolation mechanism.

### LockerService enforces a series of rules to further avoid security exploits:
- JavaScript ES5 strict mode is automatically enabled. 
- Libraries that do not support strict mode will not work with LockerService.
- Content Security Policy (CSP), unsafe-eval and unsafe-inline are disallowed. 
- Libraries using eval() or inline JavaScript code execution will not work with LockerService.


