#### What is a Vector Space 
* A **vector space** (or **linear space**) is a set of vectors that satisfies certain properties under vector addition and scalar multiplication.
* Vector Addition and Scalar Multiplication when performed on a vector produces the various linear combinations which are all a part of that vector's subspace 

#### What is a Column Space 
* The **column space** (also called the **range** or **image**) of a matrix A is the set of all possible linear combinations of its columns. 
* It represents the subspace of R^m (if A is an m×n matrix) that the matrix maps vectors into.
* Key Properties:
	- The column space is a **subspace of R^m
	- The **rank** of A is the **dimension** of the column space.
	- The column space tells us which **vectors the system Ax=b can reach**.

#### What is a Null Space 
* The **null space** (also called the **kernel**) of a matrix A consists of all solutions to the **homogeneous system** Ax=0.
* It describes the set of vectors that are mapped to the **zero vector** by A
* **Key Properties:**
	- The **null space is a subspace of R^n
	- The **dimension** of the null space is called the **nullity** of A.
	- The null space tells us about the **solutions to Ax=0**

#### Rank-Nullity Theorem 
* It describes the fundamental relationship of 
	* Rank(A) + Nullity(A) = n (number of columns)

**NOTE - The null space and the column space are sub-spaces of a vector**

**NOTE - Union of two vectors may not always give the subspace of those vectors but Intersection of the two vectors always gives us the subspace of those vectors** 