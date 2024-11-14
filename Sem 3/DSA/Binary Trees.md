#### Types of Binary Trees

* **Full Binary Tree**
* **Complete Binary Tree**
	* Height is always **log(n)**
* **Perfect Binary Tree**
* **Degenerate Binary Tree**

#### Array Implementation of Binary Trees
![[Pasted image 20241013102314.png]]
* Formulae (Array starting from 0th index)
	* Left child of index **i** would be at **[(2*i)+1]**
	* Right child of index **i** would be at **[(2*i)+2]**
	* Parent of index **i** would be at (floor)**[(i-1)/2]**
**Note - The binary tree must always be represented as a Complete Binary Tree**