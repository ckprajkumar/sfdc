# Code Convention
## General Standards

- Declare all variables at the beginning of each class. 
- All instance variables must be declared either private or protected. 
- A class must have limited public variables except 'final' or 'static final'. Try to avoid public variables where possible 
- To convert a primitive type to String use String.valueOf() instead of var + "" 
- Avoid any type of hard coding in the code. Use Custom Labels or static Apex Classes to drive any configuration related values 
- Explicitly initialize local and instance variables to their default values. 
  - String: default value is null 
  - Integer: default value is 0
  - Double: 0.0 or else it assigns null by default 
  - Boolean: false 
  - Any other Object: null 
  - Date: null 

- Do not instantiate any object before you actually need it. Generally, consider the scope of the variable and minimize where possible 
- 	All constants must have uppercase names, with logical sections of the name separated by underscores. 
- Local variables must not have the same name as instance variables. In general, variables in a nested scope must not have the same name as variables in outer scopes. 
- Use sets, maps, or lists when returning data from the database. This makes your code more efficient because the code makes fewer trips to the database. Avoid writing a SOQL inside a loop. 
- All classes that contain methods or variables defined with the webService keyword must be declared as global. If a method, variable or inner class is declared as global, the outer, top-level class must also be defined as global.

## Comments

Comments in code are important because they allow other developers to quickly comprehend the logic and intent of unfamiliar code. Avoid sentence fragments. Start sentences with a properly capitalized word, and end them with punctuation.

All source code files (of all languages and/or resource types) should have a comment block at the top that specifies the name of the class (or interface, etc.), what it does, when it was created, and who created it. If you make significant changes to a file that someone else originally wrote, add yourself to the author line.

All methods should have comments explaining what they do, including information about the input parameters and return data (if applicable). Comments should be added within methods whenever the logic or functionality of a section of code is not immediately obvious.

Block comments are used to provide descriptions of files, methods, data structures and algorithms. Block comments inside a function or method must be indented to the same level as the code they describe.

Block comment for data structures and algorithms:
```sh
/* Start Version: X.X
   * Block comments with details of changes 
   * .. 
   * .. 
   */
..
..
/* End Version: X.X */
```
Single line short comments can appear on a single line indented to the level of the code that follows.
```sh
if (condition) {
// Handle the condition.
...
}
```

Very short comments can appear on the same line as the code they describe.
```sh
if (a == 2) {
return TRUE;  // special case	
}	else {	
return isPrime(a); // works only for odd a	
}
```

### 6.2.1.	Apex
```sh
int i = 1; // This comment is ignored by the parser

int i = 1; /* This comment can wrap over multiple lines without getting 
                      interpreted by the parser. */
```

### 6.2.2.	VisualForce
```sh
<apex:page standardController="Contact">   <!-- Here is a comment -->
 <apex:sectionHeader title="Contact Edit" subtitle="New SFDC99 Member"
/>
```

### 6.2.3.	Javascript
```sh
/**

*	Returns an object as a JSON string 

*	
*	@method toJSON 
*	@return {Object} Copy of ... 
*/
```

### 6.2.4.	CSS
```sh
{
color: red;
/* This is a single-line comment */ text-align: center;
}
/* This is a multi-line comment */
```


