#### Analysis Framework
* The resources are measured either using **Theoretical Analysis** or **Experimental Study**
* Investigation of algorithms's efficiency with respect to two resources 
	* **Time** Complexity
		* The time required to execute the program is called time complexity 
		* **T(P) = C + TP(I)**
			* **(C) is the compile time** 
				* Independent of instance characteristics 
			* **TP(I) us the run or execution time**
	* **Space** Complexity
		* The memory required to execute the program is called space complexity 
		* **S(P) = C + SP(I)**
			* **(C) is the fixed space requirements**
				* Independent of the characteristics of the inputs and outputs
				* Space for simple variables, fixed-size structured variable constants
			* **(SP(I)) is the variable space requirements**
				* Dependent on the instance characteristic I
				* Number, size, values of inputs and outputs associated with I
				* Recursive stack space, formal parameters, local variables