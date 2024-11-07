* There are 2 main operations carried out by the memory
	* **Load**
		* Memory contents remain unchanged 
	* **Store**
		* Memory contents are rewritten
* An instruction is a command to the processor to perform a given task on data operands
* A program is a set of instructions that specify the operations, operands and the sequence in which the instructions need to be executed
* A computer supports 4 categories of operations
	* **Data Movement** - Control I/O transfer
	* **Data Storage** - Across memory and processor register
	* **Data Processing** - Arithmetic and logical operations 
	* **Program Sequencing and Control** - Test and Branch

#### Register Transfer Notation
* Right hand side of an RTN always represents the value whereas the left hand side represents the location where the value is to be place, overwriting the older contents in that location
	* **R1 <- [LOC]**
		* This means the value stored in LOC gets transferred to R1 register
	* **MOVE LOC, R1**
		* This is the assembly way of representing the same RTN as above
		* Contents of LOC are moved to R1 register
* The arrow notation is called Register Transfer Notation

#### Assembly Language Notation
* RTN can be converted to Assembly easily
	* **R1 <- [R1] + [R3]**
		* Equivalent assembly instruction is **ADD R3, R1**
		* R1 stores the sum

#### Basic Instruction Types
* **Three Address Instruction**
	* ADD P, Q, S
	* OPCODE | Op 1 Address | Op 2 Address | Op 3 Address (Destination)
* **Two Address Instruction**
	* ADD P, Q
	* OPCODE | Op 1 Address | Op 2 Address (Destination)
* **One Address Instruction**
	* The requirement of second operand is fulfilled by an implicit processor register called **Accumulator (AC)** 
	* ADD Q
		* AC <- Q
		* AC <- [AC] + [Q]
		* Q <- [AC]
* **Zero Address Instruction**
	* Uses the computer processor's inbuilt **pushdown stack** to implicitly carry out operations

#### Instruction Execution and Straight Line Sequencing 
* Instructions are executed one by one and the PC keeps track of the order in which the instruction needs to be executed 
* Executing an instruction is a two-phase procedure
	* First phase is called **instruction fetch** from memory
		* Instruction is fetched and placed in the IR
	* Second phase is called **instruction execute**
		* Contents of IR are decoded and processor carries out the operation
* When the execution phase is complete, the PC already has the address to the next instruction ready to be executed
* This is the straight line sequencing 

#### Branching 
* They are those instructions which change the normal sequence of execution
* There are two types of branching 
	* **Conditional Branching**
		* Changes the sequence only when certain conditions are met
	* **Unconditional Branching**
		* Changes the sequence of execution irrespective of condition of the results
* Works like If-else ladder 

#### Condition Codes
* The processor keeps track of information about the results of various operations for use by using conditional branch instructions
* Individual bits called condition code flags are set to 1 or 0 depending on the outcome of the operation performed
	* **N** (negative) - Set to 1 if the result is negative, otherwise 0
	* **Z** (zero) - Set to 1 if the results is zero, otherwise set to 0
	* **V** (overflow) - Set to 1 if the arithmetic overflow occurs, otherwise 0
	* **C** (carry) - Set to 1 if a carry-out results from the operation, otherwise set to 0