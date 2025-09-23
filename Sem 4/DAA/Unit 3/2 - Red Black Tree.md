* It is a category of the self-balancing binary search tree
* A red-black tree is a binary tree where a particular node has color as an extra attribute, either red or black
* These colors are used to ensure that the tree remains balanced during insertions and deletions

#### Properties of RBT
* Every node is colored, either red or black
* **The root is always black**
* **Every leaf is black**
* If a red node has children then the children are always black
* Black height of the RBT is the number of black nodes on a path from the root node to a leaf node. Leaf nodes are also counted as black nodes
	* **RBT of height of h has black height >=h/2**
* Height of RBT with n nodes is **h <= 2log(n+1) base2**
* **No 2 consecutive red nodes. Parent and Child node are always black if the node is red**
* New node inserted is always red
* Height of RBT **log(n) <= h <= 2log(n)**