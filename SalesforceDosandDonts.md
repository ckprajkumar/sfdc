# Salesforce  Do’s covers the following areas:
### Configuration                      
- Custom Fields
- Record Types
- Page Layouts
- Validation Rules
- Workflow Rules

### Code
- Apex Triggers
- Apex Classes
- Unit Test Classes

# Salesforce Do’s - Configuration

|Area   |Best Practice Recommendation  | Cool  |
| ------------- |:-------------:| -----:|
| Custom Objects| Use naming conventions for Custom Object APIs to identify which Customer is using  the object.This helps Orgs housing multiple business units to quickly identify who is using each custom object|Medium|
|  Custom Objects |Include Descriptions for each custom object identifying its purpose  |Medium|
|Custom Fields|Use naming conventions for all custom field APIs to identify which “group” created the field.This allows orgs with multiple business units to easily identify what “belongs” to another group, reducing the risk of other groups making changes that could negatively impact others in the org|High|
|Custom Fields|Fields Maintained by integrations. Indicate in the description text the source of the data|High|
|Custom Fields|Read Only Fields.If the data comes from an external system identify the system in the Help Text If the data is updated on another record, indicate in the help text where |High|
|Record Types|Use a naming convention that identifies which group created/uses the record type and add descriptions for each record typeEasily identifies who uses each record type||||





