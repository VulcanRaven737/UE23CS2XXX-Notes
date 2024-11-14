#### Assembly Language 
* Mnemonics are abbreviations that represent operation code of an instruction in a compact and meaningful way
* A complete set of mnemonics, symbol names for registers and memory locations and a list of rules for their use forms a programming language called Assembly Language 
* The translator is called Assembler. It converts ASM to Machine Language

#### Assembler Directives 
* Assembler Directives are the assembler commands to the assembler concerning the program being assembled. These commands are not translated into machine opcode or assigned any memory. They are essentially instructions to the assembler itself. This is what separates it from regular instruction 
* An ASM program is said to be complete if it has necessary ASM directives 

#### Assembly and Execution of Programs
* Assembler scans through the source program and it keeps track of all names and numerical values that correspond to them in a symbol table
* When a name appears a second time, it is replaced with its value from the table
* If a name is called before it has been assigned a value, a different approach is adopted by the assembler ie, it scans through the program a second time. This is called a two-pass assembler 
	* During the first pass, it creates a complete symbol table. At the end of the pass, all names would be assigned some numerical value
	* On the second pass, the assembler substitutes for all names from the symbol table
* Assembler can detect syntax errors

#### Number Notation
* There are 3 ways of representing decimal numbers in ASM
	* **ADD #83, R1**
		* Using immediate symbol, the value is directly added by the assembler as a decimal number
	* **ADD #%01011101, R1**
		* Binary conversion of the decimal number can be used as well. It must be prefixed with % symbol
	* **ADD #$5D, R1**
		* Hexadecimal conversion of the decimal number can be used as well. It must be prefixed with $ symbol
		* The first ten patterns 0000, 0001...1001 are represented by the digits 0-9 as in BCD while the remaining six 4-bit patterns are represented by A-F **(16 bits in total)**
