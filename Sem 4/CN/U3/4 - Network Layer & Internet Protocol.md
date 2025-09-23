#### Network Layer Services
* Transports segment from the sending to the receiving host
	* Sender - Encapsulates segments into data-grams and passes it to the link layer
	* Receiver - Delivers segments to the transport layer protocol
* 2 Key functions of the Network Layer
	* **Routing** 
		* It is the process of building the forwarding table by discovering and maintaining the best paths to different network destinations
		* Uses routing protocols like OSPF to exchange information with other routers
		* This happens in the background and not per packet
		* It can have 2 control planes for running routing algorithms
			* **Traditional** **Control Plane**
				* Routing algorithms run within each router communicating via routing protocols
			* **Software Defined Networking Control Plane (SDN)**
				* Logically centralized controller computes the routing tables and distributes them to routers. Router just performs routing and forwarding
	* **Forwarding**
		* It is the actual process of moving a packet from one interface to another inside a router or a switch
		* It uses the pre-built forwarding table
		* This happens per packet and the router checks the destination and sends it out to appropriate interface
#### How is it different from Transport Layer?
* In Network Layer, before any datagram is sent a virtual connection has to be established
* Routers actively participate in setting up this path
* In transport layer, communication is just between processes but in Network Layer communication is between different routers
* Hence it is different from what TCP does

#### Router Architecture
* **Input Ports** 
	* Performs Lookup
		* Determine output port based on packet header fields
		* **Destination Based Forwarding** 
			* Uses Destination IP Address. Forwarding table uses prefixes
		* **Longest Prefix Matching Rule**
			* If multiple prefixes match the destination addresses, uses the entry with the longest matching prefix, crucial for CIDR
		* **Implementation**
			* Hardware Lookup
	* Sends packets towards the Switching Fabric via a queue
	* Also checks and updates IP header fields like TTL and checksum
* **Switching Fabric**
	* Connect input ports to output ports
	* **Switching via Memory**
		* Traditional CPU copies packets from input buffer to memory then to output buffer
	* **Switching via Bus**
		* Input port puts packets onto a shared bus
		* All outputs see it but only the designated output port accepts it 
		* Gets bottle-necked by a single bus speed
	* **Switching via Interconnection Network**
		* Uses a grid of buses
		* Allows multiple packets to be transferred in parallel, if they are going to different output ports
* **Output Ports**
	* Receives packets from switching fabric and queues them
	* Performs packet scheduling and reorders packets if they are out of order
	* Queues if link is busy
* **Routing Processor**
	* Performs control plane function (runs routing protocols, communicates with SDN controller)

#### The Internet Protocol (IP)
* IP provides the best effort delivery of datagrams
* IPv4 Datagram Format
	* **Version (4 Bits)**
	* **Header Length (4 Bits)**
	* **Type of Service (8 Bits)**
	* **Datagram Length (16 Bits)**
	* **Identifier (16 Bits), Flags (3 Bits), Fragmentation Offset (13 Bits)**
	* **Time to Live (8 Bits)**
	* **Protocol (8 Bits)**
	* **Header Checksum (16 Bits)**
	* **Source IP Address (32 Bits)**
	* **Destination IP Address (32 Bits)**
	* **Options (Variable)**
	* **Data (Payload)**

#### IPv4 Datagram Fragmentation
* Different links have different Maximum Transmission Units (MTUs)
* A large IP Datagram might not fit in the MTU of a link
* Hence the router fragments an oversized IP datagram into smaller fragments
* Reassembly is done at the final destination not by intermediate routers
* Fragmentation Fields in the IP Header contain
	* **Identifier (16 Bits)**
		* All fragments of a datagram share the same identifier number
	* **Flags (3 Bits)**
		* One bit indicates if more fragments follow (1 = More flags, 0 = Last flag)
		* Another bit indicates 'do not fragment'. Router must drop the packet if its too large and this bit is set to 1
	* **Fragmentation Offset (13 Bits)**
		* Specifies where this fragment's data fits within the original datagram's payload
* Fragmentation is costly and could lead to loss of entire datagram if even a single fragment gets lost
* IPv6 avoids fragmentation

#### IPv4 Addresses 
* Each interface needs an IP address to be recognized by the router or host
* It is a 32 bit unique identifier for an interface, written in dotted decimal notation