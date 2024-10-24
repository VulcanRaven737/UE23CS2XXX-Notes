* An instruction consists of 2 parts, 
	* Operation Code
	* Operand/s
#### Registers 
* A register is a small, fast storage location within the CPU used to temporarily hold data and instructions during processing 
* Features include 
	* **Fast Access** - They are located directly within the CPU
	* **Limited Size** - Have a very small storage capacity, typically 32 bit or 64 bit depending on architecture  
	* Temporary Storage - Used to store data temporarily for fast retrieval during computations
* Types of Registers 
	* **Instruction Register (IR)**
		* Temporarily holds the instruction currently being executed by the processor
		* Decodes the instruction allowing the processor to understand and execute the required operation
	* **Program Counter (PC)**  
		* Tracks the address of the next instruction to be executed in the program sequence 
		* Automatically increments to point to the next instruction address 
	* **General Purpose Registers (R0 - Rn-1)**
		* Temporarily store operands for quick access during instruction execution. Supports various arithmetic operations and data transfer 
		* Also hold the data during calculations to improve CPU performance 
	* **Memory Address Register (MAR)**
		* Hold the memory address of the location that the CPU is about to read from or write to
		* Communicates directly with memory bus for fetching or storing data
	* **Memory Data Register (MDR)**
		* Contains the actual data being transferred to or from the addressed memory location
		* Acts as a buffer between the CPU and memory

#### Instruction Execution Cycle
* **Fetch** 
	* CPU retrieves the next instruction from memory to 