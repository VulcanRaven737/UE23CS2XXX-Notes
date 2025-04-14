#### Registers 
* General purpose registers hold either data or an address
* All the registers in ARM processor are **32 bit** 
* They **18 active registers** out of which **16 are data registers** and **2 are status registers**
	* Data Registers available are visible as R0 to R15 
	* R13, R14 and R15 are assigned special functions or a particular task
		* **R13** - Traditionally used as the stack pointer (**sp**) which stores the head of the stack in the current processor mode 
		* **R14** - Called the link register (**lr**) and its where the core puts the return address every time subroutine is called 
		* **R15** - It is the program counter (**pc**) which contains the next address to be fetched by the processor
	* R16 and R17 behave as status registers CPSR (Current Program Status Register) and SPSR (Saved Program Status Register) respectively 

#### Current Program Status Register
