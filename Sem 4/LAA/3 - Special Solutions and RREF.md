#### What is a special solution
* When we fix the value of free variables to the identity values (0 or 1) and compute the solutions of the remaining variables using it, we obtain the special solution to that subspace
* The **null space** (or **kernel**) of A consists of all solutions to Ax=0
- If the system has **free variables** (because A has more columns than pivot columns), then we can express the solutions in terms of these free variables.
- By setting one free variable to **1** and all others to **0**, we obtain a **special solution** that represents a basis vector for the null space.

#### What is RREF 
* Reduced Row Echelon Form is a simplification of the the regular echelon form
* It is obtained by manipulating the rows above the pivot to 0 using row transformations and further converting all the pivots to 1 by division
* The special solution to the vector is hidden in RREF itself
* [Identity * Free] is the form in which the RREF is in and the solution is [-Free / Identity]

