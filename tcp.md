#### TCP
Transmission Control Protocol provides reliable, ordered, and error-checked delivery of a stream of bytes between applications running on hosts communicating via an IP network.

TCP is connection-oriented, and a connection must be established between client and server before data can be sent. The server must be listening passively for incoming requests from clients beforea connection is established. 

TCP can detect when IP packets encounter errors with delivery and can request re-transmission, can re-arrange out of order packets and can ease congestion. This can lead to potentially seconds long waits for data transfer.

TCP Header
 <table>
  <tr>
    <th colspan=2>Source Port</th>
    <th colspan=2>Destination Port</th>
  </tr>
  <tr>
    <th colspan=4>Sequence Number</th>
  </tr>
  <tr>
    <th colspan=4>Acknowledgement Number</th>
  </tr>
  <tr>
    <th colspan=1/6>Offset</th>
    <th colspan=1/6>Reserved</th>
    <th colspan=1/6>Flags</th>
    <th colspan=2>Window</th>
  </tr>
  <tr>
    <th colspan=2>Checksum</th>
    <th colspan=2>Urgent Pointer</th>
  </tr>
  <tr>
    <th colspan=4>TCP Options</th>
  </tr>
</table> 

**Three-Way Handshake**  
Before data can be transmitted a connection must be established, this achieved via a three-way handshake:

For a connection intiated by host A to host B
1. (A -> B) A synchronise request (SYN) is sent with an intial sequence number (SEQ).
2. (A <- B) An acknowledgement (ACK) is returned by B along with its own SEQ.
3. (A -> B) A responds to this SYN with an ACK.


**Sequence & Acknowledgement Number**  
A sequence number (SEQ) is sent along with a chunk of bytes and the destination confirms it was recieved by sending an acknowledgement number (ACK) back to the sender. So a sequence  number of 1001 with 200 Bytes of data will be acknowledged with 1201 as the acknowledgement number i.e. ACK = SEQ + Number of Bytes sent.
The next sequence number will be 1201 i.e. SEQ increments by the number of bytes sent.

As TCP is bidirectional both client and server will transmit SEQ and ACK in each packet they send.

**Retransmission**  
When bytes are sent TCP caches the bytes that were sent and starts a Retransmission Timeout. If no ACK is recieved then after the timeout expires the bytes are re-transmitted from the cache with a new SEQ. If ACK is recieved then the cache is flushed and the timeout reset.

If the bytes were recieved but no ACK message is returned then the data is also re-transmitted. The reciever in this scenario has a duplication of that particular packet

**Delayed Acknowledgements**  
Sending 100 SEQ would require 100 ACK, in order to reduce the load on the network, delayed acknowledgements were developed, where an ACK is only sent for every other SEQ. Thus 100 SEQ would only require 50 ACK. For odd numbers of SEQ there is timeout of 500ms after which ACK is returned.

**Window Size**  
The window size determines how much data the sender can transmit. A window size of 500 bytes means that after sending 500 bytes the sender must stop transmitting and await ACK. Afer receiving ACK the sender can send another 500 bytes. The window size is set by the receiver. The window size is included in the header of every segement and is also established in the initial handshake. Window size can also be modified at any time to ease congestion and aid in flow control.

**Closing a Connection**  
Connections are terminated via a four-way closure. For host A terminating a connection to host B:
1. (A -> B) A sends a FIN flag along with a final SEQ.
2. (A <- B) B return an ACK 
3. (A <- B) B sends a FIN along with a final SEQ.
4. (A -> B) A responds to the FIN with a final ACK.
These steps may not occur sequentially and steps 2 & 3 may or may not occur together.

The reset flag (RST) can also be used to terminate a connection. If there is an error with the connection an RST flag can be sent and the connection is immediately terminated without any further ACK or SEQ messages.

**TCP Flags**  
Flags are used to communicate additonal information and are 1 bit encoded:

1. **CWR** - Congestion Window Reduced, an acknowledgement of a received ECE flag.
2. **ECE** - ECN Echo, indicates that the connection is experiencing congestion and flow control measures should be implemented. (ECN = Explicit Congestion Notification).
3. **URG** - Urgent, the packet should be processed by the receiver as a matter of priority.
4. **ACK** - Acknowledgement, Indicates that the acknowledgent header field contains meaningful data.
5. **PSH** - Push, a request for immediate data delivery  e.g. video streaming.
6. **RST** Reset, a request for the connection to be terminated when there is an error.
7. **SYN** - Synchronization, indicates that a packet is a SYN packet.
8. **FIN** - Finish, a request for the connection to be terminated after all data has been sent.




