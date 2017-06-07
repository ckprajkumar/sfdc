# Do and Don’ts :
1.	Do not write query inside a loop
2.	To update, insert and delete list of objects rather than single object
3.	Use Map to store query results
4.	The list and map can contain more than 1000 objects but be care full as if you want to display that List in VF page, it will throw error, so VF page still contains the limit of 1000 element list
5.	Use joins to query related object
6.	Do not hardcode the record type id, profile id in a properties/Util Apex class.
7.	In test method do not write direct select query use Insert first and then update or delete to cover the code.
8.	Always use batch of triggered records like Trigger.new[i].
9.	Write your test coverage class once you are done unit testing don’t wait to come back later to complete the coverage
10.	Reuse SOQL queries on a trigger, by this means don’t fire a query on an object again and again, try to optimize it.
11.	It’s better to flush all the records to storage, instead of cumulating to collection, and get a new set of data again store. This will avoid the chances of java Heap error if numbers of records store in collection are more than heap memory.
12.	Use static variables to make sure triggers are not executed multiple times ,if necessary
13.	Do not depend on a setter method being evaluated before a constructor.
14.	Methods, including methods in a controller, action attributes, and expressions, may be called more than once. Do not depend on evaluation order or side-effects when creating custom methods in a controller or controller extension.
15.	Use variables in triggers that are configurable to active/inactive triggers in production(helpful during datamigration as well).
16.	Minimum testcoverage expected for agencyfees is 85%

# Sample Code Snippets

####	Considerations Querying Large Data Set 

Use of Limit Clause - For example, if the results are too large, the syntax below causes a runtime exception:
```
//A runtime exception is thrown if this query returns enough 
records to exceed your heap limit.

Account[] accts = [SELECT id FROM account];


// Use this format for efficiency if you are executing DML statements 
// within the for loop.  Be careful not to exceed the 150 DML statement limit.

for (List<Account> accts&nbsp;: [SELECT id, name FROM account
                            WHERE name LIKE 'Acme']) {
    // Your code here
    update accts;
    }
```

###	Use of @future
```
{
    //By passing the @future method a set of Ids, it only needs to be
    //invoked once to handle all of the data. 
    
    asyncApex.processAccount(Trigger.newMap.keySet());
} 

global class asyncApex {
 @future 
 public static void processAccount(Set<Id> accountIds) {
 List<Contact> contacts = [select id, salutation, firstname, lastname, email from   Contact where accountId IN&nbsp;:accountIds];
 for(Contact c: contacts){
 System.debug('Contact Id[' + c.Id + '], FirstName[' + c.firstname + '],LastName[' + c.lastname +']');
 c.Description=c.salutation + ' ' + c.firstName + ' ' + c.lastname;
    }
    // DML Operations goes here     
  }
} 
```
### 	Batch Script Apex Class:
```
/* Batch Apex Invocation */
     public void CallingBatchJob()
     {
     
    Boolean runApex = true;
    
    BatchClassInitializer batchClassVariable = new BatchClassInitializer ();
    
    // Main Query Locator
    
    batchClassVariable.query = // Get Query Parameter From Custom Settings;
	
	// Getting Custom Setting Batch Size Value
	List<BatchRuleSize__c> batchparam = BatchRuleSize__c.getall().values();
    Integer scope_size = // Getting Batch Size From Custom Settings if batch value do not need to be defaulted to 200
        
        
        // Check if process is running..
for(AsyncApexJob job :[Select Id, Status,JobType,MethodName From AsyncApexJob where JobType = : 'BatchApex' AND MethodName =: // Method Name used for Innvocation]) 
        {
        	runApex = false;
        }
        
        if(runApex)
        { 	
         
       // Execute batch apex 
        ID batchprocessid = Database.executeBatch(batchClassVariable,scope_size);
	}
Else{
	// Else do some error processing
	}
}
 ```
 
 ### Batch Apex Class:
```
global class 
BatchClassName implements Database.Stateful,Database.Batchable<sObject>
{
	public String query;
	// Declaration of Stateful variables for remembering states
	
	global database.querylocator start(Database.BatchableContext BC)
	{
		return Database.getQueryLocator(query);
	}	
	// Main Execute Method
	global void execute(Database.BatchableContext BC, List<sObject> scope)
	{

	// Main Batch Job Processing Logic
	
	}
global void finish(Database.BatchableContext BC)
	{
	// Finish Method used for Updating/Sending Completion Email, also used for catching exceptions	

}	
```
### Update/Insert/Upsert in Apex:

All DML should be enclosed with Exception block:

```
Try{
Database.SaveResult[] sresults = Database.insert(recordSet,false);
If(Database.SaveResult result : sresults)
{
	If(result.isSuccess())
{
	// Process Success Operations
}
Else{
	// Process Error Operations
}
}
}
Catch(Exception e)
{
	// Process Exception Blocks
}
```



