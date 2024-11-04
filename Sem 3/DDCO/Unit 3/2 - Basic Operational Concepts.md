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
	* CPU retrieves the next instruction from memory using the PC
*  **Decode**
	* CPU interprets the instruction to determine which operation needs to be performed and what data it needs to use 
* **Execute**
	* CPU carries out the instruction 

#### Instruction Execution Process
* **Programs reside in memory**
	* Programs are loaded into the computer memory from input devices like keyboards. This is before the program execution starts
* **PC is set to point to the first instruction**
	* Program Counter holds the address of the first instruction which is to be executed
* **Contents of PC are transferred to MAR**
	* Memory Address Register holds the address of the memory location that is being accessed. Address is copied from the PC
* **Read Signal Passed**
	* A signal is sent to read data from the address stored in the MAR
* **Instruction is read and loaded into MDR**
	* Instruction located at the address is read from memory and placed into 
* **Contents of MDR are transferred to IR**
	* Instruction Register holds the current instruction to be executed. 
* **Instruction is ready to be decoded**
	* Instruction in the IR is decoded and executed 
* **Operations are performed by the ALU if mentioned**
	* If the instruction requires ALU ops, then the operands are obtained
		* If the operands are stored in the general purpose registers they are directly fetched
		* If the operands are stored in the memory, address of the operand is transferred to MAR, and a read signal is passed to fetch the operand. The operand is read into MDR after fetching and transferred to the ALU
* **ALU finishes any operation to be carried out using the instructions and operands
* **Store the result back**
	* If the result needs to be stored in the general purpose register, it is directly stored
	* If the result needs to be stored in the memory, MAR receives the address and the result is written to MDR. A write signal is passed to memory to store the result
* **During the execution of the current process, the PC increments to the next instruction**

#### Interrupt
* An interrupt is a request from an I/O device for service by the processor
* Normal execution of instructions are interrupted if some device required urgent servicing