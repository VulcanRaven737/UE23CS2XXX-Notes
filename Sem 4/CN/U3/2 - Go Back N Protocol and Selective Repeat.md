* In the GBN protocol, the sender is allowed to transmit multiple packets without waiting for an acknowledgment, but is constrained to have no more than some max allowable number N of unacknowledged packets in the pipeline
	* **Sender has a window size of N**
	* **Receiver has a window size of 1**
* In Selective Repeat, the sender is allowed to send N packets in a window. **Both sender and receiver have a window size of N.** If a packet is missed and next packet already arrives, it is put into a buffer and ACK is sent for every packet thereafter
	* When the lost packet is re-transmitted, the ACK for it is sent and is delivered to the upper layer
	* The lost packet is delivered to the upper layer, after which the buffered packets are sent 
	* As a rule of thumb, **Sequence Number >= 2 X Window Size**
![[Pasted image 20250415123655.png]]