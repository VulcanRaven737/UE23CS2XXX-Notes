#### ARM7TDMI - 3 Stage
* ARM7 follows the 3 stage pipeline process 
	* **Instruction Fetch (IF)**
		* The instruction is fetched from memory and placed in the instruction pipeline
	* **Instruction Decode (ID)**
		* The instruction is decoded and the datapath control signals are prepared for the next cycle 
	* **Instruction** **Execute (EX)**
		* The register bank is read, an operand shifted, ALU carries out the processing and is written back into a destination register 

#### ARM9TDMI - 5 Stage
* ARM9 follows the 5 stage pipeline
	* **Instruction Fetch (IF)** 
	* **Instruction Decode (ID)** 
	* **Instruction Execute (EX)** 
	* **Memory Access (MEM)** 
	* **Writeback (WB)**