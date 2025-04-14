#### Building a Reliable Data Transfer Protocol
![[Pasted image 20250414141011.png]]
* **rdt1.0**
	* In this channel we assume it to be completely reliable
	* The sender and receiver FSMs have only a single state
	* The receiver is able to receive data as fast as the sender happens to send data
![[Pasted image 20250414140932.png]]
* **rdt2.0**
	* This is a more realistic model in which the bits in a packet maybe corrupted
	* These bit errors occur in the physical components of a network as a packet is transmitted
	* ARQ (Automatic Repeat reQuest) protocol is used for this channel
		* **Error Detection**
		* **Receiver Feedback**
			* **ACK** or **NAK** replies are sent by the receiver to the sender in response to every packet it receives
		* **Re-transmission**
			* A packet that is recieved in error will be re-transmitted by the sender
	* Send side of FSM has 2 states. It receives the data to be transmitted from the application software, creates a packet and sends it to the receiver. In this process it moves to the 2nd state where it waits for ACK or NAK. In this state it cannot get more data from the application
	* It follows the stop and wait protocol
	* We haven't accounted for the fact that the ACK or NAK packet could be corrupted
![[Pasted image 20250414141854.png]]
* **rdt2.1**
	* It overcomes the issue of sending duplicating packets in rdt2.0, by introducing sequence numbers for every packet sent
	* The sequence number is either a 0 or a 1
	* The ACK or NAK packet need not include this sequence number 
![[Pasted image 20250414142956.png]]
![[Pasted image 20250414143000.png]]
* **rdt2.2**
	* It ditches the NAK acknowledgement and instead adopts only ACK
	* Additionally the receiver must also now attach the sequence number with the ACK packet ie **- ACK, 0 or ACK, 1**
![[Pasted image 20250414143357.png]]
![[Pasted image 20250414143719.png]]
* **rdt3.0**
	* In addition to corrupted bits, the channel is also losing packets. This is tried to overcome using rdt3.0
	* This model introduces a timer, which upon expiry automatically re-transmits the packet
		* Expiry could be caused due to delay in ACK to be received from the receiver or if a corrupt bit was transmitted
		* It could also be caused if an out of order packet was sent
![[Pasted image 20250414144542.png]]
#### Piplelined Reliable Data Transfer Protocols
* rdt3.0 is a successful and functionally correct protocol but it is slow due to its stop-and-wait architecture
* Hence to improve performance, pipelining is carried out
![[Pasted image 20250414144921.png]]
* Go-Back-N and Selective Repeat are the two protocols used for pipelining RDT
