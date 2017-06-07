# Configuration Standards
[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)
## Object Standards
 -	Before creating an object you must check that if a standard object or an existing custom object can be used for the same                 requirement or not.
-	Object name should be Self- Explanatory, why this is created for.
-	You must always provide the detail description while creating the object.
-  Sharing model should be default private for all objects, make use of public group sharing and manual sharing to create data visibility, unless explicit business needs to make it public available for all roles across the org.

## Field Standards

-	Before creating a field, check that if an existing custom field can be used for the same requirement or not.
-	Field name should be less than 40 characters and Self- Explanatory, why this is created for.
-	Field description should follow following pattern <Module Name>#<GEO>#<FIELD USAGE>#<Functional Description>
        - where GEO = APAC,EMEA, AMER or GLOBAL
	- Field usage : If the field is used by integration points then mention the integration      service as well , for example :              INTG_APRIMO . If the field is used by more than one integration points than you can expand the definition as            		 INTG_SYS1_INGT_SYS2. If the field is used for data migration from other system then tag the field      as DATAMG
	 Example:   MJA#EMEA#INTG_SIEBEL#DATAMG#. The field captures whether the account is major account or not and the value flows from Siebel to SFDC for EMEA.

- Consult with object owners before creating the fields.

## Page Layouts
-	To reduce the number of page layouts to maintain, use the same page layout for all profiles for a specific record type.
-	Use field-level security to restrict users’ field access; use page layouts primarily to organize pages.



## Validation Rules Standards
-	When creating validation rules, consider all the settings in your organization that can make a record fail validation such as assignment rules, field updates, field-level security, or fields hidden on a page layout.
-	Be careful not to create two contradicting validation rules for the same field; otherwise, users will not be able to save the record.
-	A poorly designed validation rule can prevent users from saving valid data. Make sure you thoroughly test a validation rule before activating it. Users will never be able to save a record if your formula always returns a “True” value.
-	Write helpful error messages: Always include the field label. 
-	Because users may not know what field is failing validation, especially if your error message is located at the top of the page?
-	Give instructions.
-	An error message like “invalid entry” does not tell them what type of entry is valid. Use an error message like "Close Date must be after today."
-	If users in your organization speak different languages, translate your error messages using the translation workbench. 
-	Consider what fields are visible and editable for users on their page layouts due to field-level security. Fields that are not visible or editable for the user are still available in the formula for a validation rule and can cause a validation error.
-	Use the record type ID merge field in your formula to apply different validation for different record types.
-	When using a validation rule to ensure that a number field contains a specific value, use the ISNULL function to include fields that do not contain any value. For example, to validate that a custom field contains a value of '1,' use the following validation rule to display an error if the field is blank or any other number: OR(ISNULL(field__c), field__c<>1)

## Workflows and Approval Process Best Practices:
-	Avoid associating more than one field update with a rule or approval process that applies different values to the same field.
-	You can't convert a lead that has pending actions ( generated from time dependent workflow).
-	Design workflow actions so that you can use them for both workflow rules and approval processes.
-	After creating approval processes, use the graphical Process Visualizer to visualize and understand the defined flow and decisions.
## Views, Reports and Dashboard Standards
-	Report name or View name should clearly indicate what its contents.
-	Reports must be segmented and stored in separate folder based on their purpose and module.
-	Reports to be used organization wide should be kept in unified public folder. As reports in personal folder will not be available for public usages. 
-	Report name should postfix with “-Dashboard” if its part of dashboard.

## Generic Configuration considerations
Following generic practices should be followed while customizing Salesforce.com:
-	Creation of profiles should be restricted, make use of existing profiles with different page layouts, record types, and control data visibility using private data model
-	Creation of roles should be restricted, make use of public groups to share common data access/visibility
-	Naming conventions should be followed for creation of any profiles, roles etc
-	 Custom Object should be given for garbage collection if not being used across by any of project teams, make use of Salesforce.com Destructive APIs to clean them up as part of deployment, same thing applies to fields on standard object or custom object.
-	Fields should be created with default no visibility for any profiles and roles, and access only should be given to roles and profiles depending on specific business needs.
-	Make use of workflows, rollup summary and formula fields where ever possible instead of triggers.
-	Creation of Custom Label should also follow naming convention, as Custom Label file as part of deployment is one across the whole org.

