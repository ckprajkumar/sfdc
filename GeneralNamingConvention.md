# General Naming Convention

All descriptions and comments must be written in English. We want to ensure that the Force.com Org is self documented. Where description fields are available in the standard configuration it will be required to be completed.

  - Aside from loop iterators such as i, j, and k, variable names, object names, class names, and method names should always be descriptive. Single-letter names are not acceptable
- Class names should use CapitalizedCamelCase. Method or Function names should use lowerCamelCase. Constants should be CAPITALIZED_WITH_UNDERSCORES.
-   Underscores should not be used for any variable name, object name, class name or method name – with the exception of to supply the suffices detailed below.
-   Abbreviations should be avoided for all names due to risk of misinterpretation.
-   Overriding standard tabs, objects and standard names is not allowed without Customer IT team exception.
-   Description fields for all objects, fields, and other metadata should be populated where provided.
-   Negated variable names must be avoided.
-   Avoid the use of Static variables wherever possible.
-   SFDC Keywords cannot be used as Class variables.
-   Name must be meaningful. Abbreviations in names must be avoided. Like:


```sh
computeAverage(); // NOT: compAvg();
```


```sh
boolean isError; // NOT: isNoError
```
See more on [ProjectSpecificNamingConvention.md](https://github.com/ckprajkumar/sfdc/blob/master/ProjectSpecificNamingConvention.md)