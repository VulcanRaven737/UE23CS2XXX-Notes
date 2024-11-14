#### Single Bus Arrangement 
* Multiple IO devices may be connected to the processor and the memory via a single bus 
* Bus consists of 3 sets of lines to carry 
	* **Address**
	* **Data**
	* **Control Signals**
* To access an IO device, the processor places the address in the address lines. Each IO device has a unique address

#### Accessing IO Devices
* There are two types 
	* **IO Devices and the memory share the same address space**
		* Called **Memory Mapped IO**
		* Any machine instruction that is accessible by the memory can also be accessed by the IO device
	* **IO Devices and the memory share different address space (Intel Architecture)**
		* Special instructions to transfer data to and from IO devices and memory
		* IO and memory share the same address lines. They have different address spaces which can be accessed using control signals
* IO device is connected to the bus using an IO interface circuit which has the following connected to it 
	* **Address Decoder** 
		* Decodes the address placed on the address lines, enabling the device to recognize the address
	* **Data Register**
		* Hold the data being transferred to or from processor 
	* **Status Register**
		* Hold information that is necessary to perform the IO operation
* Data and Status register are connected to data lines and have unique addresses
* All of these comprise of the device's interface circuit

#### Program Controlled IO
* 4 registers involved in the data transfer operations are 
	* **DATAIN**
	* **DATAOUT**
	* **STATUS** -  Consists of 4 more flags (KIRQ and DIRQ are used in conjunction with interrupts)
		* **SIN**
		* **SOUT**
		* **KIRQ**
		* **DIRQ**
	* **CONTROL**
* To sync data transfers between the IO devices we use the status flags to monitor the data flow
* There are 2 more mechanisms to achieve the synchronizing data transfers between the processor and memory 
	* **Interrupts**
	* **Direct Memory Access**

#### Interrupts 
* When the processor continuously monitors the status of the device, it does not perform any useful tasks
* An alternate approach would be for the IO device to alert the processor when it becomes ready 
	* It does so by sending a hardware signal called an interrupt to the processor 
	* An interrupt-request line is dedicated for this purpose
* Processor can perform other useful tasks while it is waiting for the IO device to get ready
* Routine executed in response to an interrupt is called the **interrupt-service** routine 
* When an interrupt occurs, control must be transferred to the interrupt-service routine 
* Before transferring control occurs, the content of the PC (i+1) are saved in a known location for easy retrieval in the future
* Return address, or the contents of the PC are usually stored on the processor stack
* This enables the **return-from-interrupt** instruction to resume execution at (i+1) location

#### Interrupts vs Subroutine
* A subroutine performs a task that is required by the calling program 
* Interrupt-service routine has nothing in common with the program that was being executed before it got interrupted by the routine
* Interrupt-service routine and the program it interrupts may belong to different users

#### Interrupt Latency 
* Saving and restoring information can be done automatically by the processor but saving and restoring registers involved memory transfers 
	* This increases the total execution time 
	* Increasees the delay between the time an interrupt request in received and the start of the interrupt-service routine. This delay is called **Interrupt Latency**
* To reduce this latency most processors only save minimal amount of information
	* This includes the PC and processor status registers
* Any other information that needs to be saved has to be explicitly mentioned at the beginning of the interrupt service routine

#### Interrupt Enable and Disable 
* Interrupt requests interrupt the execution of a program which may alter the intended sequence of events 
	* Sometimes these alterations are not desirable and should not be allowed 
* Processors provide the ability to enable and disable such interrupts 
* **Interrupt-enable** and **Interrupt-disable** serves this purpose

#### Avoidance of successive interrupts 
* Interrupt-service signal is active only until it learns that the processor has responded to the request
* It is important that the active signal does not lead to multiple/successive interrupts
* **The processor automatically disables interrupts before starting the execution of the current interrupt-service routine**
* **The processor has a special interrupt request line which allows only one signal to be passed even if the line is active. This is called edge triggered**

#### Sequence of handling interrupts
* The IO device raises an IR 
* Processor interrupts the program it is currently executing 
* Interrupts are temporarily disabled 
* The IO device is informed that its request has been recognized and in response also disables the IR signal
* The action requested is carried out by the interrupt-service routine (ISR)
* Interrupts are again enabled and the interrupted program is resumed 

#### Handling multiple devices 
* Each IO device operated independently and hence no definite order is imposed
* When a request is received over the common interrupt-service line, additional information is needed to identify the particular device that activated the line 
	* The additional information is available in the STATUS registers, KIRQ and DIRQ
	* The ISR **polls** all the IO devices connected to the bus 
	* The first device with its **Interrupt Request Bit** set, is the device which needs to be serviced 
	* The issue with polling is that, it requests for the IRQ bits of all the IO devices, regardless of whether they are asking for service. This slows the process down
	* This is overcome using **Vectored Interrupts**

#### Vectored Interrupts
* A device requesting for service or an interrupt may identify itself directly to the processor 
* The IO device requesting for an interrupt can identify itself by sending a special 4-8 bits long, code to the processor over the bus
* The code supplied may be the starting address of the ISR for that specific device. This means the code is unique for every device and must be used every time that device required servicing 
* The interrupting device puts the data on the bus only when the processor is ready to receive it 
* When the processor is ready to receive the data, it activates the **interrupt_acknowledgment** line **(INTA)**
* The IO device responds to the INTA by sending the **interrupt_vector** code. This deactivates the **interrupt_request** **(INTR)** signal

#### Interrupt Nesting
* To avoid delaying the IO device from receiving addressing from the processor, a priority order is set up ie: IO devices with higher priority get addressed first even when the device with lower priority is being addressed 
* This priority can be overridden only when the processor is being run in the **supervisor mode**. Such an interrupt is called **privilege exception**

#### Simultaneous Requests 
* Arrival of interrupt requests simultaneously demands for a solution. Which IO device must the processor service first?
* If the processor has individual interrupt request lines, then the IO device with the higher priority is addressed first 
* But if the processor has a single, common interrupt request line then 
	* Polling the status registers of the IO devices. Priority is determined by the order in which the devices are polled
	* Vectored interrupts using daisy chain

#### Daisy Chain 
* The interrupt request line INTR is common to all devices 
* The interrupt acknowledgment line is connected in a daisy fashion such that the INTA signal propagates serially through all devices
* When INTR is made active, the processor responds by setting the INTA to 1 
* The signal is recieved by the first device (closest to the processor) and then passes it to the next device only if it doesn't require any servicing 
	* But if the device does require servicing then, it blocks the INTA signal and proceeds to put its vector code on the data lines
* Hence the device closest to the CPU gets serviced first 

#### Exceptions
* Exception is any event that causes in interruption
* Interrupt requests from IO devices are also a form of exception
* Exceptions are called when 
	* Recovery from errors
	* Debugging 
	* Privilege Exception
* When errors are detected 
	* Processor performs the same steps as in the case of interrupt service request
	* It suspends the execution of the current program and starts executing an **exception service routine**
* The only differences between the two is that 
	* Exception service routine, when triggered, causes the processor to immediately abandon the current program being executed 
	* Interrupt service routine when triggered does not have a very high priority, hence the processor delays the request by a bit, by completing the current program being run 