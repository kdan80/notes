<style>
table, th, tr {
    border: none;
    outline: none;
    border-collapse: collapse;
    text-align: center;
}

td {
    border-top: none;
    border-bottom: none;
}

.tab-bg {
    background: #2a2a2a;
    
}
</style>

#### TCP
Transmission Control Protocol provides reliable, ordered, and error-checked delivery of a stream of bytes between applications running on hosts communicating via an IP network.

TCP is connection-oriented, and a connection must be established between client and server before data can be sent. The server must be listening passively for incoming requests from clients before a connection is established. TCP can be thought of as occuring over 3 phases:
1. Establishing a connection (three-way handshake).
2. Data transfer.
3. Terminating a connection (four-way closure).

TCP takes data from the application layer and encapsulates it in a TCP header and then transmits it as a byte stream to the internet layer. 

Some of the responsibities of TCP include, flow control, congestion control, error handling and reliable, ordered and guaranteed delivery of data. As such it is used where reliable data transfer is essential e.g. file transfers, web browsing and email. Its strict adherence to data integrty means that is slower than other protocols with potentially seconds long waits for data.

A TCP segment is composed of a variable length header (20 - 60 bytes) and data in the form of a byte stream.

Structure of a TCP header
 <table class="tab-bg">
  <tr>
    <td colspan=2>Source Port</td>
    <td colspan=2>Destination Port</td>
  </tr>
  <tr>
    <td colspan=4>Sequence Number</td>
  </tr>
  <tr>
    <td colspan=4>Acknowledgement Number</td>
  </tr>
  <tr>
    <td colspan=1/6>Data Offset</td>
    <td colspan=1/6>Reserved</td>
    <td colspan=1/6>Flags</td>
    <td colspan=2>Window</td>
  </tr>
  <tr>
    <td colspan=2>Checksum</td>
    <td colspan=2>Urgent Pointer</td>
  </tr>
  <tr>
    <td colspan=4>TCP Options</td>
  </tr>
</table> 

**Three-Way Handshake**  
Before data can be transmitted a connection must be established, this achieved via a three-way handshake:

For a connection intiated by host A to host B
1. (A -> B) A synchronise request (SYN) is sent with an intial sequence number (SEQ).
2. (A <- B) An acknowledgement (ACK) is returned by B along with its own SEQ.
3. (A -> B) A responds to this SYN with an ACK.

**Source Port (16 bits)**  
Identifies the sending port.

**Destination Port (16 bits)**  
Identifies the receiving port.

**Sequence Number (32 bits)**  
Has a dual role:
* If the SYN flag is set then this is the initial sequence number.
* If it is not set then this is the accumulated sequence number.

**Acknowledgement Number (32 bits)**  
If the ACK flag is set then this number is the next sequence number that is expected by sender of the ACK. It acknowledges receipt of all prior bytes that were sent. If it is not set this number is meaningless.

**Data Offset (4 bits)**  
It specifies the length of the header (in 32 bit words) and thus where the data starts. The minimum header size is 5 (equal to 20 bytes) and the maximum is 15 (equal to 60 bytes). So a maximum header size allows for an additonal 40 bytes of information to be transmitted.

Note: A 4 bit number can have 2<sup>4</sup> = 16 values i.e. 0 to 15.

**Reserved Bits (6 bits)**  
Not currently used and reserved for future use. Should be set to 000000 and ignored by the receiver.

**Flags (8 bits)**  
Flags are used to communicate additonal information and are 1 bit encoded:

1. **CWR** - Congestion Window Reduced, an acknowledgement of a received ECE flag.
2. **ECE** - ECN Echo, indicates that the connection is experiencing congestion and flow control measures should be implemented. (ECN = Explicit Congestion Notification).
3. **URG** - Urgent, urgent pointer is valid and the packet should be processed by the receiver as a matter of priority.
4. **ACK** - Acknowledgement, Indicates that the acknowledgent header field contains meaningful data.
5. **PSH** - Push, immediately pass data to the application layer.
6. **RST** Reset, a request for the connection to be terminated when there is an error.
7. **SYN** - Synchronization, indicates that a packet is a SYN packet.
8. **FIN** - Finish, a request for the connection to be terminated after all data has been sent.

**Window Size (16 bits)**  
The size of the receive window, which specifies the number of window size units that the sender of this segment is currently willing to receive. Used for flow and congestion control.

**Checksum (16 bits)**  
Used for error-checking of the TCP header, the payload, and the IP pseudo-header.

**Urgent Pointer (16 bits)**  
If URG flag is set all data up to and including the end of the sequence number plus the urgent pointer is considered urgent and is pushed to the application via a separate IPC channel (SIGURG). Not recommended due to interoperability concerns across operating systems.

**Options (0 to 320 bits, in 32 bit chunks)**  
The length of this field is determined by the data offset field. It is used to send an additional options and information, including but not limited to:
* Maximum Segment Size (MSS) - Used by the client during the handshake to inform the server of the packet size it can receive. SYN only.
* Window scale - Used for increasing window size beyond 2<sup>16</sup>. SYN only.
* Selective ACK (SACK) - Informs the sender about multiple packets lost, so that the sender can recover the loss asap. SACK is setup during the initial handshake.
* TCP Fast Open (TFO) - Used to establish a faster conenction setup without the need for a 3-way handshake. The first connection still requires a 3WH but all subsequent connections use a TFO cookie. SYN only (the TFO option is SYN only, TFO cookie is used on for all other request from the client).
* Timestamps - Used to compute the round trip time for packets i.e. network latency. This can then be used to set a timeout to determine when packets should be resent.
* No Option (NOP) - Used to align options to 32-bit boundaries for better performance.

**Padding**  
Used to ensure that the TCP header ends, and data begins, on a 32-bit boundary. Comosed of zeros.

**Sequence & Acknowledgement Number**  
A sequence number (SEQ) is sent along with a chunk of bytes and the destination confirms it was recieved by sending an acknowledgement number (ACK) back to the sender. So a sequence  number of 1001 with 200 Bytes of data will be acknowledged with 1201 as the acknowledgement number i.e. ACK = SEQ + Number of Bytes sent.
The next sequence number will be 1201 i.e. SEQ increments by the number of bytes sent.

As TCP is bidirectional both client and server will transmit SEQ and ACK in each packet they send.

**Retransmission**  
When bytes are sent TCP caches the bytes that were sent and starts a Retransmission Timeout. If no ACK is recieved then after the timeout expires the bytes are re-transmitted from the cache with a new SEQ. If ACK is recieved then the cache is flushed and the timeout reset.

If the bytes were recieved but no ACK message is returned then the data is also re-transmitted. The reciever in this scenario has a duplication of that particular packet

**Delayed Acknowledgements**  
Sending 100 SEQ would require 100 ACK, in order to reduce the load on the network, delayed acknowledgements were developed, where an ACK is only sent for every other SEQ. Thus 100 SEQ would only require 50 ACK. For odd numbers of SEQ there is timeout of 500ms after which ACK is returned.



**Closing a Connection**  
Connections are terminated via a four-way closure. For host A terminating a connection to host B:
1. (A -> B) A sends a FIN flag along with a final SEQ.
2. (A <- B) B return an ACK 
3. (A <- B) B sends a FIN along with a final SEQ.
4. (A -> B) A responds to the FIN with a final ACK.
These steps may not occur sequentially and steps 2 & 3 may or may not occur together.

The reset flag (RST) can also be used to terminate a connection. If there is an error with the connection an RST flag can be sent and the connection is immediately terminated without any further ACK or SEQ messages.




**Window Size (16 bits)**  
The window size determines how much data the sender can transmit. A window size of 500 bytes means that after sending 500 bytes the sender must stop transmitting and await ACK. Afer receiving ACK the sender can send another 500 bytes. The window size is set by the receiver. The window size is included in the header of every segement and is also established in the initial handshake. Window size can also be modified at any time to ease congestion and aid in flow control.

