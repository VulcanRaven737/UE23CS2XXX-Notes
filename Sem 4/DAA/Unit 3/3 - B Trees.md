**B - Trees are multi-way search trees and they are height balanced**
* All nodes must be at the same level
* Every node must have atleast (degree/2) number of children
#### 2-3 Trees
* There are 2 keys per node with max 3 possible children per node
* While adding nodes, split the node and grow the tree upwards
* While deleting
	* **Simply Delete**
	* **Borrow**
	* **Delete and Merge**
* Min nodes in a 2-3 tree is given by
	* **n = 2^(h+1) - 1**
	* **max height = logbase2(n-1) - 1**
	* **O(logn)base2**
* Max nodes in a 2-3 tree is given by 
	* **n = 3^(h+1) - 1 / (3-1)**
	* **min height = logbase3(h (3-1) + 1) - 1**
	* **O(logn)base3**