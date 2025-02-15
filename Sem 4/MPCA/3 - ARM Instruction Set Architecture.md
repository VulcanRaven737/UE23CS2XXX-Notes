#### Features 
* All ARM instructions are 32 bit wide 
* It uses load store architecture 
* It has 3 address data processing instructions, 2 source operands and 1 result operand register 
* Conditional execution is followed for every instruction 
* It has the ability to perform general shift operation and ALU operation in a single instruction that executes in a single clock cycle

### Load-Store Architecture 
* LOAD instruction operations apply to the memory state which copies values from memory into the register 
* STORE instructions copy register values into memory 
* ARM does not support memory-to-memory operations 
* 3 types of ARM instructions 
	* Data Processing instructions 
		* **MOV, ADD, SUB, CMP**
	* Data Transfer instructions 
		* **LDR, STR**
	* Control Flow instructions 
		* **B, BGT, BEQ, BL**

### RISC features rejected by ARM
* **Register Windows**
	* RISC incorporated a large number of registers which reduced the data traffic between the processor and memory resulting from register saving and storing 
	* The issue with this was that the chip area occupied a large amount of space due to the registers and hence this feature was rejected by ARM on cost grounds
* **Delayed Branches**
	* Branches cause pipleline problems since they interrupt the smooth flow of instructions
	* Hence RISC processors started using delayed branches where the branch takes effect after the following instructions finishes execution 
	* This does not work well with super scalar implementation and hence was dropped in ARM
* **Single-Cycle execution of all instructions**
	* ARM executes most data processing instructions in a single clock cycle while other tasks take multiple clock cycles 
	* Single cycle operation of all instructions is only possible with separate data and instruction memories which is not the case with ARM

#### ARM Instructions 
* ARM instructions are grouped as 
	* **Data Processing Instructions** 
		* MOV, ADD, SUB, CMP, CMN
	* **Branch Instructions** 
		* B, BL, BGT, BLT, BGE, BLE
	* **Data Transfer Instructions** 
		* LDR, STR, LDM, STM
	* **Software Interrupt Instructions** 
		* SWI 0x11
	* **Program Status Register Instructions** 
		* MSR, MRS