# Best Practices for Designing Bulk Programs
The following are the best practices for designing bulk programs:
-	Minimize the number of data manipulation language (DML) operations by adding records to collections and performing DML operations against these collections.
-	Minimize the number of SOQL statements by preprocessing records and generating sets, which can be placed in single SOQL statement used with the IN clause.

## Apex Scheduling Best Practices
-	Salesforce.com only adds the process to the queue at the scheduled time. Actual execution may be delayed based on service availability. 
-	Use extreme care if you are planning to schedule a class from a trigger. You must be able to guarantee that the trigger will not add more scheduled classes than the ten that are allowed. In particular, consider API bulk updates, import wizards, mass record changes through the user interface, and all cases where more than one record can be updated at a time. 
-	Though it's possible to do additional processing in the execute method, Salesforce.com recommends that all processing take place in a separate class.
-	You can only have ten classes scheduled at one time. You can evaluate your current count by viewing the Scheduled Jobs page in Salesforce.com or programmatically using the Force.com Web services API to query the AsyncApexJob object.

## Batch Apex Best Practices 
-	Use extreme care if you are planning to invoke a batch job from a trigger. You must be able to guarantee that the trigger will not add more batch jobs than the five that are allowed. In particular, consider API bulk updates, import wizards, mass record changes through the user interface, and all cases where more than one record can be updated at a time.
-	When you call Database.executeBatch, Salesforce.com only places the job in the queue at the scheduled time. Actual execution may be delayed based on service availability. 
-	When testing your batch Apex, you can test only one execution of the execute method. You can use the scope parameter of the executeBatch method to limit the number of records passed into the execute method to ensure that you aren't running into governor limits. 
-	The executeBatch method starts an asynchronous process. This means that when you test batch Apex, you must make certain that the batch job is finished before testing against the results. Use the Test methods startTest and stopTest around the executeBatch method to ensure it finishes before continuing your test. 
-	Use Database.Stateful with the class definition if you want to share variables or data across job transactions. Otherwise, all instance variables are reset to their initial state at the start of each transaction. 
-	Methods declared as future are not allowed in classes that implement the Database.Batchable interface. 
-	Methods declared as future cannot be called from a batch Apex class. 
-	You cannot call the Database.executeBatch method from within any batch Apex method. 
-	Each batch Apex invocation creates an AsyncApexJob record. Use the ID of this record to construct a SOQL query to retrieve the job’s status, number of errors, progress, and submitter. 
-	For a sharing recalculation, Salesforce.com recommends that the execute method delete and then re-create all Apex managed sharing for the records in the batch. This ensures the sharing is accurate and complete.