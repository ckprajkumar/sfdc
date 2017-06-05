# 8. Development Processes
-	All code must be held within the Customer Source code management system. 
-	All code must be auditably peer reviewed by a senior developer. 
-	All commits need comments including User Story ID and description of the work performed. 
-	The source control repo follows the Git Flow branching model, as follows: 

`Picture to be Updated`

-	Development work starts by creating and committing to Feature branches from the Develop branch. 
-	When Feature branch code is ready and peer reviewed, a Pull Request is requested to merge it into the Develop branch. 
-	Once merged into the Develop branch the Code will be quality checked and build verified and unit tested in a Continuous Integration Sandbox. 
-	Following successful automated quality checks a Pull Request is used to merge the Develop branch into a Master branch. This branch will be used as the deploy version into QATest, PreProd and Production. 