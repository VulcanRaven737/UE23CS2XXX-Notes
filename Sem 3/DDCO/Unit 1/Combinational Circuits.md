#### Adders
* **Half Adder**
	* Used for numbers having 2 bits only
* **Full Adder** 
	* Used for 3 or more bits
	* Created using series of Half Adders
![[Pasted image 20240910190741.png]]
* **4-Bit Binary Ripple Carry Adder**
	* Formed using a combination of full adders
![[Pasted image 20240910190702.png]]
**Note : For an n-bit adder, there are 2n gate levels for the carry to propagate from input to output**

* **Carry Look-ahead Generator**
	* It overcomes the carry delay by avoiding having to wait for the carry to ripple through all the Full Adders
	* It generates the carry-in of each full adder simultaneously without causing any delay.
![[Pasted image 20240910221948.png]]
* **Binary Adder-Subtractor**
	* A-B can be implemented by taking the 2's complement of B and adding it to A
	* For performing subtraction value of M should be equal to **1**
	* For signed int
		* if MSB is 1 = -ve
		* if MSB is 0 = +ve
	* If V = 0, no overflow, otherwise overflow of bit occurred
	* -ve number has to be represented in 2's complement
![[Pasted image 20240910221928.png]]
#### Decimal Adder 
* **BCD Adder**
	* Numbers till 9 can be represented natively in Binary Coded Decimal
	* To represent numbers beyond 9, we must add 6 (0110) to the sum and represent it in binary
![[Pasted image 20240910225013.png]]
#### Magnitude Comparator
* It is a circuit that compares two numbers A and B and determines their relative magnitudes
* The outcome indicates if A>B, A=B or A<B. 
* **The circuit for comparing two n-bit numbers has 2^2n entries in the truth table making it cumbersome**
* The algorithm is a direct comparison between individual pair of bits for the 2 numbers
* X-NOR is used to check equality of bits 
![[Pasted image 20240912154245.png]]
#### Decoders 
* A binary code of n bits is capable of representing up-to 2^n distinct elements of coded information
* A decoder converts binary information from n input lines to a maximum of 2^n unique output lines
* Used for binary to octal conversion
#### Encoders 
* Used for octal to binary conversion