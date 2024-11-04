* Main memory is a collection of semiconductor storage cells
* Each cell can store one bit of information either **1** or **0**
* Memory stores binary information in groups of bits called **words**
* The length of the memory word can be specified as n-bits per words called the word length
* Computers have a word length of 16 bits to 64 bits
* Each word is identified uniquely using an **Address**. Special input lines called **Address Lines** select each and every word in memory.
* Each word in memory is assigned an **identification number** as well starting from 0 to 2^k where k is the number of address lines
* The most practical assignment is to have successive addresses refer to successive byte locations in the memory (**Byte Addressable Memory**)

**Note - 1 Byte = 8 Bits**
**For a 16 Bit processor - Word length is 2 Bytes
For a 32 Bit processor - Word length is 4 Bytes
For a 64 Bit processor - Word length is 8 Bytes**

#### Big Endian Assignment
* When lower byte addresses are used for the more significant bytes of the word, it is called big endian assignment
* The most significant value in the sequence is first at the lowest storage address
* Basically stores the most significant btt first, hence data is read from left to right

#### Little Endian Assignment 
* When lower byte addresses are used for the least significant bytes of the word, it is called little endian assignment
* The least significant value in the sequence is first at the lowest storage address
* Basically stores the least significant bit first. hence data is read from right to left

**Note - Endianness refers to the order in which bytes are arranged in memory**