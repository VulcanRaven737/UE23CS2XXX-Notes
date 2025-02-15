* An operating system is a software that manages a computer's hardware. 
* It provides a basis for application programs and acts as an intermediary between computer user and the computer hardware

#### What Operating System do?
* A computer system can be divided roughly into four components 
	* Hardware 
		* CPU
		* Memory
		* IO Devices 
	* Operating System
	* Application Programs
	* User
* The OS features a **kernel** which is the code that runs at all times
* It then includes the **middleware** as well, which is a set of software frameworks which aid with the development of application programs 
* It also includes the **system programs** which aid in the working of the OS but may not be running at all times
* An operating system is interrupt driven 
* Interrupt transfers control to the interrupt service routine through the interrupt vector which contains the addresses of all the service routines 

### Interrupt Handling 
* The operating system saves the state of the CPU by storing registers and the program counter 
* Separate segments of code determine what action should be taken for each type of interrupt

### Storage Structures 
* RAM is the main memory which the CPU can access directly. It is volatile though
* The processor fetches instructions from memory, decodes and executes them 
* The fetch, decode, execute cycles are repeated until the program terminates. This is called Von-Neumann model of computing
* ROM is the non volatile, secondary storage device which is used 
* Caching means copying information into a faster storage system. RAM can be seen as a cache for 
* Cache memory is always smaller than the memory from which data is being referenced from
* Cache is the faster memory available after registers 