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
##### DOM Manipulation Libraries
Using jQuery, you would:
- Assign a unique id to the input element
- Use a jQuery selector to find the input element in the DOM
- Assign the value to that DOM element

[Image]

Using the Lightning Component Framework, you would:
- Declare a score attribute
- Bind the input field to the score attribute
- Set the value of the score attribute: the bound input field will automatically display the new value.


