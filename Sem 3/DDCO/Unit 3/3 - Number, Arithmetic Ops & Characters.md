#### Number Representation
* There are 3 major representations
	* **Sign and Magnitude**
	* **One's Complement**
	* **Two's Complement**
* Leftmost bit is **0** for positive and **1** for negative

#### 1's Complement
* In 1's complement, -ve value is obtained by complementing each bit of the corresponding +ve
* There are 2 representations of 0, which causes problems
![[Pasted image 20241104190955.png]]
#### 2's Complement
* Obtained by doing 1's complement first and then adding 1 to it
* Only one representation of 0 but there is an extra negative number than positive number ie. -8, 7
![[Pasted image 20241104191140.png]]
#### Addition and Subtraction of Numbers
* For addition, regular addition of digits is carried out
* For subtraction, 2's complement of the number which is being subtracted, is found and added with the other number

#### Overflow in Integer Arithmetic
* Overflow occurs only when two numbers having same signs are added
* **An overflow is detecting by checking the signs of X, Y and the result. If the result does not have the same sign as X and Y, then an overflow occurred**

#### Characters
* Characters can be letters of the alphabets, decimal digits, punctuation marks etc 
* They are represented by codes that are usually eight bits long ie: ASCII
