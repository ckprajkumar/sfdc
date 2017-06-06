# Code Convention
### General Standards

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
