#### Instructions 
* Move Instructions
	* MOV - Move 
	* MVN - Move and Negate 
* Arithmetic Instructions
	* ADD - (R0 = R1 + R2)
	* ADC - (R0 = R1 + R2 + C)
	* SUB - (R0 = R1 - R2)
	* SBC - (R0 = R1 - R2 + C - 1)
	* RSB - (R0 = R2 - R1) [Reverse Subtraction]
	* RSC - (R0 = R2 - R1 + C - 1)
* Compare Instructions 
	* CMP - Compare 
	* CMN - Compare and Negate
* Bit-wise Logical Operations
	* AND - (R0 = R1 AND R2)
	* ORR - (R0 = R1 OR R2)
	* EOR - (R0 = R1 XOR R2)
	* BIC - (R0 = R1 AND (NOT R2)) [Bit Clear]

#### Condition Codes 
* Any data processing instruction can set the condition codes 
	* ADDS R2, R2, R0
	* ADC R3, R3, R1

#### Shifted Register Operands 
* One operand to ALU is routed through the Barrel Shifter. Thus the operand can be modified before it is used
* It is useful for fast multiplication and dealing with lists, table and other complex data structures
* Barrel Shifter functions 
	* **Logical shift left** 
		* One left shift diagonal line is turned on 
	* **Logical shift right** 
		* One right shift diagonal line is turned on
	* **Arithmetic shift left** 
		* Uses sign extension rather than zero filling for unconnected output 
		* After shifting the bits left, it fills the rightmost bits with 0s
		* Works for both signed and unsigned numbers 
	* **Arithmetic Shift right***
		* **ASR (Arithmetic Shift Right)** shifts bits **right**, but unlike LSR, it preserves the **sign bit (MSB - Most Significant Bit)**.
		- This makes **ASR useful for signed numbers**, as it performs **signed division**
	* **Rotate Operation**
		* The right shift diagonal is enabled together with complementary left shift diagonal
		* Rotate Right (ROR) moves bits to the right and wraps the bits that fall off back to the leftmost position.  

#### Multiplication using Barrel Shifter 
* **Multiplication by 2^n**
	* MOV Ra, Rb, LSL #n
		* n = 0 means multiplying by 1 
		* n = 1 means multiplying by 2 
		* n = 2 means multiplying by 4 and so on
* **Multiplication by (2^n)+1**
	* ADD Ra, Ra, Ra, LSL #n 
		* n = 0 means multiplying by 2
		* n = 1 means multiplying by 3 
		* n = 2 means multiplying by 5 and so on 
* **Multiplication by (2^n)-1**
	* RSB Ra, Ra, Ra, LSL #n 
		* n = 0 means multiplying by 0
		* n = 1 means multiplying by 1 
		* n = 2 means multiplying by 3 and so on

#### Division using Barrel Shifter 
* **Division by 2^n**
	* MOV Ra, Rb, LSR #n
		* **n = 0** means dividing by **1** (no change)
		- **n = 1** means dividing by **2**
		- **n = 2** means dividing by **4**
		- **n = 3** means dividing by **8**
- **Division by (2^n)+1**
	- ADD Ra, Ra, Ra, LSR #n
		- **n = 1** means dividing by **3** (approx)
		- **n = 2** means dividing by **5** (approx)
		- **n = 3** means dividing by **9** (approx)
- **Division by (2^n)-1**
	- RSB Ra, Ra, Ra, LSR #n
		- **n = 0** means dividing by **0** (invalid)
		- **n = 1** means dividing by **1** (no change)
		- **n = 2** means dividing by **3** (approx)
		- **n = 3** means dividing by **7** (approx)