#### Standard IO Interfaces 
* IO device is connected to a computer using an interface circuit 
* Standard Interface Circuits are developed with standardized protocols
* A motherboard houses the processor chip, main memory and some additional IO interfaces. Devices which require high-speed connection to the processor are connected directly to the processor bus 
* Motherboard also provides another bus that can support more devices
	* The **Expansion Bus** is connected to the Processor Bus by a circuit called 'bridge'
	* Devices connected to the expansion bus experience a small delay in data transfers
* Processor bus cannot be made standardized and are unique to the architecture of the processor 
* Expansion bus on the other hand has a uniform standard defined 
	* IBM's Industry Standard Architecture 
	* There are 3 widely used bus standards 
		* **PCI** (Peripheral Component Interconnect)
		* **SCSI** (Small Computer System Interface)
		* **USB** (Universal Serial Bus)
* The bridge translates signals and protocols from the processor bus to the PCI bus or expansion bus and vice-versa

#### PCI Bus 
* **Peripheral Component Interconnect**
* Processor independent and low cost with plug and play capability
* Supports memory transfers which involve a 'burst' of data 
![[Pasted image 20241114143544.png]]
* PCI supports three independent address spaces namely 
	* **Memory**
	* **IO**
	* **Configuration**
* Master/Initiator maintains the address information on the bus only until the slave is to be selected, after which it can be freed. This consumes only a single clock cycle, thereby making the process efficient with a significant cost reduction
* The master or initiator is the processor. It issues the read and write commands
* The addressed device that responds to the read and write commands is called the target
* A complete transfer operation on the bus, involving an address and a burst of data is called a transaction
* PCI incorporates in each IO device, a small configuration ROM memory that stores information about that device
	* This ROM is accessible in the **configuration address space**. The PCI initialization software reads this ROM and determines what kind of device it is 
* There is a limit of 21 IO devices on the PCI bus before the addresses have been initialized. After initialization there can be upto 32 IO devices 

#### SCSI Bus
* **Small Computer System Interface**
* SCSI devices are not accessible by the processor and are not part of the address space. It is connected to the processor via an SCSI controller
	* This controller uses a **DMA (Direct Memory Access)** to transfer data packets from the main memory to the device or vice-versa
* SCSI also has an initiator and a target. The controller acts as a target
	* Once the connection between the initiator and the target have been established, it can be suspended or restored as required
	* When suspended, other devices can use the bus 
	* This ability to overlap data transfer requests is one of the key features of the SCSI bus that leads to its high performance
* **Events of the SCSI Controller** 
	* The SCSI controller acting as an initiator, contends for control over the bus 
	* Once it wins the arbitration process, it selects the target controller and hands over the control to it 
	* The target starts an output or input operation. In response to this the initiator sends a command specifying the required read or write operation
	* The target controller then suspends the connection with the initiator controller in order to perform a disk seek, wherein it sends a command to the disk to move its head to a certain sector, in order to read data from it. It stores this data in a data buffer. 
	* The target controller then restores the connection with the initiator controller and transfers the data buffer to it and suspends the connection again
	* It again performs the same read operation and the cycle repeats until all of the data from the data buffer has been transferred to the initiator controller. 
	* Finally the connection between the initiator and target re terminated. The initiator controller receives the data and stores it into the main memory
	* The SCSI controller sends an interrupt to the processor notifying it that the requested 
* **SCSI Data transfer stages**
	* Arbitration 
	* Selection 
	* Information Transfer 
	* Re-selection

#### USB 
* Universal Serial Bus
* USB has a tree structure 
* Each node of the tree has a hub which acts as an intermediate control point between the host and the IO device. At the root of the tree, a root hub connects the entire tree to the main computer. The leaves of the tree are the IO devices being served 
* A hub copies a message that it receives from the upstream connection to all its downstream ports. As a result all IO devices can see what message was sent by the host computer but only the addressed  IO device can respond to it.  
* The message sent by an IO device upstream is however only visible to the hub and the host computer. Other IO devices cannot see this message. Hence USB does not allow IO devices to communicate with each other 
* Each device on the USB whether the hub or the IO device is assigned a 7-bit address. This address is local to the USB tree and is not related to the addresses stored on the  processor bus
* USB root hub acts as a single device attached to the processor. The host computer communicates with the individual devices by sending packets of information via the root hub which forwards it to the apt device in the tree
* All information transferred over the USB is organized in packets where a packet consists of one or more bytes of information. The information transferred on the USB can be divided into 2 broad categories
	* **Control**
		* Perform tasks such as addressing a device to initiate data transfer, acknowledging that data has been received correctly or indicating an error 
	* Data
		* Carry information that is delivered to a device 
* A packet consists of one or more fields 
	* **Packet Identifier (PID)**
		* First field of any packet is called the **Packet Identifier (PID)**, which identifies the type of that packet. They are transmitted twice
		* 8 bit field used in all USB packets 
		* First 4 bits fields are the normal PID and the next 4 bit fields are the complement of the first 4 bits
	* **Token Packet (IN or OUT)**
		* Control data transfer operations (IN or OUT)
		* They consist of the following fields
			* **PID** - Specifies type of the packet
			* **ADDR** - Address of the device that the host is communicating with
			* **ENDP** - Endpoint number, identifies the specific endpoints within the device
			* **CRC16** - (Cyclic Redundancy Check) Error checking field, computed over the ADDR and ENDP fields to detect any transmission error
	* **Data Packet**
		* These packets carry the payload data 
		* They consist of the following fields
			* **PID** - Identifies the type of the data packet
			* **DATA** - The data being transferred ranging from 0 to 8192 bits in length
			* CRC16 - Used to detect errors
* Isochronous traffic on USB is one of the key use cases 
* USB also supports Inochronous data 