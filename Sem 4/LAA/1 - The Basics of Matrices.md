### 1. **Homogeneous System of Equations**

A system of linear equations is called **homogeneous** if all the right-hand side (RHS) values are zero:

Ax=0Ax = 0Ax=0

---

### 2. **Trivial Solution**

A **trivial solution** is the solution where all variables are set to zero:

For any homogeneous system, the trivial solution always exists.

---

### 3. **Nontrivial Solution**

A **nontrivial solution** is a solution where at least one variable is **not** zero.

For example, if we solve a system and find:

x=1,y=−2,z=3x = 1, y = -2, z = 3x=1,y=−2,z=3

then this is a **nontrivial** solution.

A homogeneous system has a **nontrivial solution** if and only if the coefficient matrix AAA is **singular** (not full rank).

---

### 4. **Coefficient Matrix**

A **coefficient matrix** is a matrix that contains only the coefficients (numbers multiplying the variables) of a system of equations.

For example, for the system:

The coefficient matrix is:

---

### 5. **Singular Matrix**

A **singular matrix** is a square matrix that **does not have an inverse**. This happens when its determinant is zero:

det⁡(A)=0\det(A) = 0det(A)=0

A singular matrix means that its rows (or columns) are **linearly dependent**, meaning one equation can be written as a combination of others.

---

### 6. **Full Rank vs. Not Full Rank**

- The **rank** of a matrix is the number of **independent** rows (or columns).
- A matrix has **full rank** if all of its rows (or columns) are independent.
- If a matrix is **not full rank**, then it has dependent rows/columns, meaning some equations do not provide new information.

For example, if we have:

Here, the second equation is just **twice** the first one, so they are **not independent**. This system is **not full rank**.

---

### 7. **Rank of a Matrix**

The **rank** of a matrix is the number of **independent** rows (or columns).

For example, in the matrix:

- The second row is just **twice** the first row.
- So, the number of **independent** rows is **2**, not 3.
- Therefore, rank(A)=2\text{rank}(A) = 2rank(A)=2.

If a matrix has fewer independent rows than the number of variables, there will be **free variables**, leading to infinitely many solutions.

---

### 8. **Free Variables**

A **free variable** is a variable that can take any value in a system of equations. This happens when there are more unknowns than independent equations.

For example, in this system:

x+y=0\begin{aligned} x + y &= 0 \end{aligned}x+y​=0​

- There are **two variables** (x,yx, yx,y) but only **one equation**.
- Here, yyy can be anything, and xxx will adjust accordingly.
- So, yyy is a **free variable**.

If a system has **free variables**, it has infinitely many solutions.

---

### 9. **Underdetermined System**

An **underdetermined system** is a system where there are **more variables than equations**.

For example:

x+y+z=0\begin{aligned} x + y + z &= 0 \end{aligned}x+y+z​=0​

Here, we have **3 variables** (x,y,zx, y, zx,y,z) but only **1 equation**. This means the system has **infinitely many solutions**.

---

### 10. **Invertible Matrix**

A square matrix AAA is **invertible** if it has an inverse, meaning there exists another matrix A−1A^{-1}A−1 such that:

AA−1=IA A^{-1} = IAA−1=I

where III is the **identity matrix**.

A matrix is **not invertible (singular)** if:

det⁡(A)=0\det(A) = 0det(A)=0

If AAA is invertible, then the homogeneous system Ax=0Ax = 0Ax=0 has only the **trivial solution** x=0x = 0x=0.

---

### 11. **Solution Space**

The **solution space** is the set of all possible solutions to a system of equations.

- If a system has **only the trivial solution**, its solution space is just the **zero vector**.
- If a system has **free variables**, the solution space forms a **subspace** of multiple possible solutions.

For example, if the solution space is:

[xyz]=c[1−23],c∈R\begin{bmatrix} x \\ y \\ z \end{bmatrix} = c \begin{bmatrix} 1 \\ -2 \\ 3 \end{bmatrix}, \quad c \in \mathbb{R}​xyz​​=c​1−23​​,c∈R

then there are **infinitely many solutions**, depending on the value of ccc.


#### Gauss Elimination Process 
* This process breaks down when the matrix is singular ie: its determinant is equal to 0
* When it breaks down it is either temporary or permanent. 
* Temporary breakdown is fixed using row exchange but permanent breakdown either has infinite solutions or no solution

### Gauss Jordan Method 
* An nxn matrix is invertible only if it has n pivots 
* Every non singular matrix is invertible 
* If inverse of a matrix exists, that matrix is also a symmetric matrix which means its equal to its transpose 

**Note - In LDU factorization, D must be invertible which means all its pivots must be non zero. Hence singular matrix do not have LDU factorization**

**Note - The rank of the matrix remains the same even when it is transposed, ie: it has the same number of pivots regardless of transpose or not**