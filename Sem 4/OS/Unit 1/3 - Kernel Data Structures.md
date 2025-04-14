* **Array** 
	* **Main memory is constructed using array** 
	* But it cannot include items of varying size 
* **Linked List** 
	* It can accommodate items of varying sizes and allows easy insertion and deletion of items 
	* Performance for retrieving a specified item in a list of size is poor as it may potentially require traversing through all the n elements 
	* It is the basis for constructing some powerful data structures like stack and queues
* **Stack**
	* Sequential DS which uses LIFO principle 
	* **Used when involving system calls** 
	* All the parameters and local variables are pushed onto the stack when a function is called 
	* When the function returns a value, all these values are popped out of the stack 
* **Queue**
	* Sequential DS which uses FIFO principle 
	* CPU scheduling makes use of queues
	* Tasks waiting to be run on available CPU are organized in queues
* **Trees**
	* DS used to represent data hierarchically
	* Values are linked through a child-parent relationship
	* Balanced Binary Tree is used by Linux for selecting which task to run next (CPU Scheduling)
* **Hash Function and Maps**
* **Bitmaps**
	* It is a strings of n binary digits representing the status of n items 
		* 0 - resource available 
		* 1 - resource unavailable
		* 001011101 means resources 2,4,5,6,8 are unavailable and 0,1,3,7 are available
	* Commonly used to represent the availability of a large number of resources on disk blocks