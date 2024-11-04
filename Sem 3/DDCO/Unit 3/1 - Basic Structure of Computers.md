#### What is Computer Organization
 * This field focuses on the functional units of a computer system and how they work together to execute instructions
 * It deals with the internal structure and operation of a computer including its I/O components like the CPU, memory and control unit
 * A computer is a sophisticated electronic calculating machine that 
	 * Accepts input information 
	 * Processes the information according to a list of internally stored instructions
	 * Produces the resulting output information

#### Types of Computers 
* **Classification 1** 
	* Micro Computers 
	* Mini Computers 
	* Mainframes
	* Super Computers 
* **Classification 2** 
	* Analog Computers 
	* Digital Computers 
	* Hybrid Computers 
* **General Classification** 
	* General Purpose Computers 
	* Special Purpose Computers

#### Workstations 
* Industry standard desktop computers with more computation power and high-resolution graphic display with a variety of graphic input/output capability 

#### Enterprise Systems or Mainframes
* They have tremendous computing power beyond workstations
* They are quite expensive and equipped with several hard disks, RAIDs and backup storage units

#### Super Computers 
* Fastest computer type currently available 
* Computation speed of a super computer is measured in terms of Floating Point Operations Per Second (**FLOPS**)

#### Functional Units 
* A computer in its simplest form comprises of 5 functional units 
	* **Input Unit** 
		* Computer accepts encoded information through input unit 
	* **Output Unit** 
		* Computer returns the computed results, error messages from the output unit
	* **Memory Unit** 
		* **Primary/Main Memory**
			* Memory that is directly accessible by the CPU
			* It comprises of DRAM and provides the actual working space to the processor
			* It provides fast access to data and instructions and allows CPU to quickly read from and write to memory
			* It does have limited memory
		* **Secondary/Auxiliary Memory**
			* Processor does not directly interact with secondary memory. It is first interfaced by the primary memory
			* It retains data even without power and has a much higher storage space than primary memory 
			* Slower access compared to primary memory
	![[Pasted image 20241021174933.png]]

	* **Arithmetic & Logic Unit** 
		* ALU is a digital circuit that performs arithmetic and logic operations on binary numbers 
		* ALU process binary data using logic gates which perform operations like AND, OR, NOT and XOR. The combination of these gates allow ALUs to execute complex arithmetic and logic operations
		* In order to execute operations/instructions, operands need to be brought into the ALU from the memory
			* Operands are stored in **general purpose registers** available in the ALU
			* Access times of the GPR are much faster than cache
	* **Control Unit** 
		* It performs the following tasks
			* **Coordinate Activities** - The CU oversees the operations of all the components within the CPU
			* **Issues Timing Signals** - It sends signals like **MEMR (Memory Read)**, **MEMW (Memory Write)**, **IOR (Input Output Read)** and **IOW (Input Output Write)**
			* **Governs Data Transfers** - These timing signals determine when specific operations should occur
			* **Interpret Instructions** - The CU decodes instructions to determine the required actions
		* Operations of Input Unit, Memory, ALU and Output Unit are coordinated by CU
	* Therefore operations of computer can be summarized as 
		* Accepts information from the input units 
		* Stores the information in memory 
		* Processes the information (ALU)
		* Provides the processed results through the output unit
		* All of these operations are coordinated by the control unit 