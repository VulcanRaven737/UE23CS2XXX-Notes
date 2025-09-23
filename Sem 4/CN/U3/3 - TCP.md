* TCP is the internet's dominant transport layer protocol providing reliable, connection oriented, full-duplex, point-to-point service
* Uses mechanisms derived from RDT (checksum, sequence numbers, ACKs, timers, retransmissions)
* It is connection oriented and requires a 3-way handshake

#### Properties 
* **Connection Oriented Service** 
	* Client and Server must perform a handshake before exchanging any application data
* **E2E Connection** 
	* The connection states reside only on the two end hosts
	* Intermediate routers are oblivious to TCP connections. They only see the IP datagrams
* **Service Characteristics**
	* Full Duplex
		* Data can flow in both directions simultaneously over the same connection
	* Point to Point
		* Connection exists only between a single sender and single receiver. Multicasting not supported
* **Data Transfer Mechanisms**
	* Send Buffer
	* Segmentation
	* Maximum Segment Size
	* Segment Creation
	* Passing to IP
	* Receive Buffer
	* Application Reading
#### TCP Segment Structure
* **Header Fields**
	* ***Source Port Number*** **(16 Bits)**
		* Has the port number of the sender
	* ***Destination Port Number*** **(16 Bits)**
		* Has the port number of the receiver
	* ***Sequence Number*** **(32 Bits)**
		* Used to keep track of the position of data
	* ***Acknowledgement Number*** **(32 Bits)**
	* ***Header Length*** **(4 Bits)**
		* Holds the length of the TCP header
	* ***Flags*** **(6 Bits)**
		* C, E - Congestion Notification
		* U, A, P, R, S, F - Urgent Pointer, ACK, Push, Reset, Sync, Finish
	* ***Receive Window*** **(16 Bits)**
		* How many bits the receiver is willing to accept
	* ***Checksum*** **(16 Bits)**
		* Used to check the integrity of the segment
	* ***Urgent Data Pointer*** **(16 Bits)**
	* **Optional Fields** **(Variable)**
		* ***Options***
			* Variable length, used for things like window scaling
* **Payload** **(Maximum Segment Size)**
	* ***Application Data***
		* Variable length, this is the actual data passed from the application

#### TCP Round Trip Time Estimation and Timeout
* TCP uses a timeout mechanism like rdt3.0 to recover from lost segments
* Usually set TimeoutInterval > RTT but not much larger 
* Estimating RTT
	* **SampleRTT** 
		* Measure time from when a segment is sent until its ACK is received
		* Measured for only one unacknowledged segment at a time
		* Sample RTT is not calculated for retransmitted segments
	* **EstimatedRTT**
		* Average of recent Sample RTT values, smoothed using Estimated Weighted Moving Average (EWMA)
		* ![[Pasted image 20250422223754.png]]
	* **DevRTT**
		* Estimate of how much SampleRTT typically deviates from EstimatedRTT using Estimated Weighted Moving Average (EWMA)
		* ![[Pasted image 20250422224122.png]]
* Setting the Timeout Interval
	* **TimeoutInterval = EstimatedRTT + 4 * DevRTT**
	* Provides a larger margin when RTT variation is high (measured using DevRTT)
	* Initial TimeoutInterval set to 1 second
* Timeout Backoff
	* When a timeout occurs 
		* Retransmits the missed segment
		* Increases the TimeoutInterval by 2x for every subsequent failure
	* Exponential backoff prevents flooding a congested network with rapid retransmissions
	* When the lost segment's ACK arrives, TimeoutInterval is recalculated 