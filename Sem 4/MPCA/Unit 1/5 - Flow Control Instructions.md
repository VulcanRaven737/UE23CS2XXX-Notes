There are two types of control instructions 
* Branch Instruction 
* Conditional Branch Instruction 
	* Branch executed only when a certain condition is satisfied 

#### Address memory locations 
* Memory is addressed by a register and an offset 
	* LDR R0, [R1]
* Three ways to specify offsets 
	* **Immediate** 
		* LDR R0, [R1, #4] @mem[R1+4]
	* **Register** 
		* LDR R0, [R1, R2] @mem[R1+R2]
	* **Scaled Register** 
		* LDR R0, [R1, R2, LSL#2] @mem[R1+4*R2]

#### Addressing Modes of ARM Architecture 
* **Preindexing of Preindexing without Writeback**
	* **LDR Rd, [Rn, OFFSET]**
	* LDR R0, [R1, R2] @ R0 = mem[R1+R2]
	* R1 unchanged
* **Preindexing with Writeback or Autoindexing** 
	* **LDR Rd, [Rn, OFFSET]!**
	* LDR R0, [R1, R2]! @ R0 = mem[R1+R2] 
	* R1 = R1 + R2 (meaning of write-back)
	* R1 updated before loading it into the destination register
* **Post Indexing** 
	* **LDR Rd, [Rn], OFFSET**
	* LDR R0, [R1], R2 @ R0 = mem[R1]
	* R1 = R1 + R2
	* R1 updated after loading it into the destination register

Note - Autoindexing and Post-indexing are the most efficient. Preindexing without write-back is the most inefficient  
Note - Preindexing is useful for accessing an element in a data structure. Autoindexing and Post-indexing are useful for traversing an array