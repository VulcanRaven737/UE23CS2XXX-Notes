#### Unconstrained Non-Linear Optimization
* **Gradient Descent**
	* Method suitable for solving unconstrained non-linear optimization problems is gradient descent 
	* It is an iterative optimization algorithm used to find the minimum of a function
	* ![[Pasted image 20241117190217.png]]

#### Constrained Nonlinear Optimization
* Can be complex due to non-linearity of the objective function and constraints
* Can be solved using the following method 
	* **Lagrange Multiplier Method**
		* Classical approach used for problems with equality constraints 
		* Transforms the constrained optimization problem into an unconstrained problems by incorporating the constraints into the objective function
		* ![[Pasted image 20241117190953.png]]
	* **Karush-Kuhn-Tucker (KKT) conditions**
		* Used to solve **Convex Optimization Problems**
			* Convex Optimization is a special case of optimization where the objective function is convex and the feasible region is defined by constraints which are also convex 
			* **A function is convex if its 2nd derivative is non-negative**
			* It ensures that any local minimum is a global minimum
			* If any objective function is convex and the feasible region is also convex, then any local minimum is also a global minimum
			* The constraints can be either linear or nonlinear but the feasible region must be a convex region
		* KKT conditions extend the method of Lagrange Multipliers to handle both equality and inequality constraints
		* Following are the KKT Conditions 
			* **Stationarity Conditions**
				* The derivative of the Lagrangian wrt x must be zero
			* **Primal Feasibility** 
				* The constraint g(x)<= 0 must be satisfies 
			* **Dual Feasibility** 
				* The Lagrange multiplier ƛ>= 0
			* **Complementary Slackness**
				* ƛ * g(x) = 0
				* 
#### Genetic Algorithms 
* **Computation Intelligence** usually refers to the ability of a computer to learn a specific task from data or experimental observation
* Computational Intelligence is a set of nature inspired computational methodologies and approaches to address real world problem to which mathematical or traditional modelling cannot be used
* **Evolutionary Algorithms** are a subset of optimization techniques inspired by the principles of natural evolution, such as **selection, mutation, crossover and survival of the fittest**
* Genetic Algorithm is a meta heuristic search based optimization technique inspired by the principles of Genetics and Natural Selection