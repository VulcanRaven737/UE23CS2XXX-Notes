* It is the **brain of the computer** and the basic computing unit on an integrated chip
* Three basic characteristics differentiate different microprocessors 
	* **Instruction Set** 
		* The set of instruction which the microprocessor can execute 
	* **Bandwidth** 
		* The number of bits processed in a single instruction
	* **Clock Speed**
		* How many instruction per second the processor can execute (MHz)
#### Microprocessor vs Microcontroller
* **Microprocessor** 
	* It accepts digital data as input, processes it according to the instruction stored in the memory and provides the results as an output 
	* It does not have RAM, ROM and other peripherals 
	* Memory, IO and other components are connected externally only 
	* High power consumption 
* **Microcontroller** 
	* It consists of the CPU and also a fixed amount of RAM, ROM and other peripherals all on a single chip
	* Designed to perform specific tasks and requires small resources like RAM, IO and can be embedded on a single chip
	* Reduces the size of the cost
	
#### Classification of Microprocessor
* **CISC** (Complex Instruction Set Computer)
	* CISC philosophy was implemented in large computers 
	* It makes the compiler development simpler and shifts most of the burden of generating machine instruction to the processor
	* Philosophy of CISC processors is to simplify the code and make it shorter in order to reduce the memory requirement 
	* In a CISC processor a single instruction has several low level operation which makes the instructions short but more complex
	* Architecture 
		* **They have variable length instruction sets with many formats** 
		* **It allows values in memory to be used as operands in data processing instructions**
	* Pentium is still considered a CISC processor and follows the x86 architecture
* **RISC** (Reduced Instruction Set Computer)
	* John Cocke is considered to be the father of the RISC architecture 
	* He proved that 20% of the instructions did 80% of the work 
	* Philosophy of RISC is to simplify operation of individual instruction. **It has separate instructions for LOAD and STORE**
	* Number of lines of code in the program increases but the amount of work done by the user is reduced 
	* Architecture 
		* **It uses a fixed 32 bit architecture**
		* **It uses a load-store architecture where instructions that process data, operate only on registers and are separate from instruction that access memory** 
		* **There is a large register bank (32 bit)**
	* It uses hard wired instruction decode logic with pipelined and single cycle execution
	* Provides higher performance with shorter development time 
	* The drawback are that there is **poor code density and it does not exist x86 code**

#### RISC vs CISC 
* Both architectures seem to have adopted the strategies of the other 
* CISC chips are now able to execute more than one instruction within a single clock cycle, including pipelining

| **RISC**                                       | **CISC**                                     |
| ---------------------------------------------- | -------------------------------------------- |
| Simple instructions and few in number          | Many complex instructions                    |
| Fixed length instruction                       | Variable length instruction                  |
| Multiple register sets                         | Single register set                          |
| Three operands per instruction                 | One or two register operands per instruction |
| Single cycle instructions                      | Multi-cycle instructions                     |
| Hardwired control                              | Micro-programmed control                     |
| Highly pipelined                               | Less pipelined                               |
| Only LOAD/STORE instructions can access memory | Many instructions can access memory          |
| Limited addressing modes                       | Multiple addressing modes                    |

#### Processor design trade-offs
* The processor spends more time moving data and doing calculations to find the new address location than actually doing arithmetic computations 
* To carry out this moving of data efficiently there are 3 different ways the processor does it 
	* **Pipelining** 
		* It is the most effective way of exploiting concurrency in a processor 
	* **Cache Memory** 
		* CPU cache is the cache used by the CPU to reduce the average time time taken to access data from the main memory 
	* **Super-Scalar instructions** 
		* A super-scalar processor executes more than one instruction during a clock cycle by simultaneously dispatching multiple instructions to different compute units on the processor 