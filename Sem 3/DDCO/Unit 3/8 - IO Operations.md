#### Program controlled I/O
* Method of I/O Operation in which the CPU is directly responsible for managing the transfer of data between peripherals 
* Processor and I/O require sync due to difference in speeds 
	* This is achieved using a buffer register 
	* The processor sends a character and waits for a signal that the character has been received

#### Mechanism of I/O transfer 
* A character key when pressed from the keyboard, its scan code is sent to an 8-bit register **DATAIN** in the keyboard
* Processor is informed about a valid character presence in the register by settings a synchronization flag **SIN** to **1** 
* I/O driver program continuously monitors the contents of SIN flag and when its set to 1, it reads the contents of DATAIN
* The character stored in DATAIN is transferred to processor over the system bus and **SIN is immediately reset to 0**
* Similar events take place while transferring data from processor to the display
* **DATAOUT** register hold a character's code to be displayed when the sync flag **SOUT** is set to 1. When SOUT is equal to 1, the display is ready to receive the next character from processor
* Transfer of a character to DATAOUT resets the flag to 0
* **Hence when DATAOUT is ready to receive instruction it is set to 1. When it holds instructions it is set to 0**
* The buffer register **DATAIN** and **DATAOUT** and control flags **SIN** and **SOUT** forms parts of connectivity circuits known as **device interface or interface hardware** 