## 1 - Networking

Networking, communication betweem computer systems, is governed by a reference model known as the Open Systems Interconnection (OSI) model. This model is split into 7 layers that define how systems should communicate with each other. The layers are:

<ol reversed>
 <li>Application layer</li>
 <li>Presentation layer</li>
 <li>Session layer</li>
 <li>Transport layer</li>
 <li>Network layer</li>
 <li>Data link layer</li>
 <li>Physical layer</li>
</ol>

Layers 7,6,5 are often grouped together as an application/software layer and its collective  purpose is to produce data that lower layers can then encapsulate. Each layer specifies its own Protocol Data Unit (PDU) that represents how data is abstracted at that layer (see Layer Architecture below). Each layer has well defined functions and methods that allow adjacent layers to interact with it. Moving down the stack data is "encapsulated" and moving up it is "decapsulated".

**Layer Architecture**
 <table>
  <tr>
    <th colspan=2>Layer</th>
    <th>Protocol Data Unit (PDU)</th>
    <th>Function</th>
    <th>Protocols</th>
  </tr>
  <tr>
    <td>7</td>
    <td>Application</td>
    <td rowspan=3>Data</td>
    <td>High-level protocols</td>
    <td>HTTP, FTP, SMB/CIFS, SSH</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Presentation</td>
    <td>Data translation; character encoding, compression and encryption/decryption</td>
    <td>TLS, SSL, JPEG, GIF, MIDI, MPEG etc</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Session</td>
    <td>Managing communication sessions</td>
    <td>RPC, NetBIOS, H.245</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Transport</td>
    <td>Segment, Datagram</td>
    <td>Data flow & control between ports/services on a network</td>
    <td>TCP, UDP</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Network</td>
    <td>Packet</td>
    <td>Data flow & control between devices on a network</td>
    <td>IPv4, IPv6, IPsec</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Data Link</td>
    <td>Frame</td>
    <td>Data flow & control between adjacent nodes on a network</td>
    <td>PPP, MAC</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Physical</td>
    <td>Bit</td>
    <td>Data flow & control over physical network equipment</td>
    <td>Bluetooth, 5G, LTE, 802.11 Wifi, USB, PCI-Express all provide physical layer services</td>
  </tr>
</table> 

**Layer 7: Application**  
OSI layer that is host-based or user-facing and directly interacts with software applications. File sharing, message handling and database connections are common examples. HTTP, FTP, SMB/CIFS, SMTP, SSH, DNS, NFS, WebDAV, WebRTC, WebSockets all work at the application level.

**Layer 6: Presentation**  
This layer establishes data formatting and translation as specified by the appliction layer and handles encryption/decryption, decompression/compression. Essentially data is transformed into the format that the application layer accepts.

**Layer 5: Session**  
This layer creates, controls and manages the lifetime of a remote connection. Check points are created so that any failure in data transmission can be restarted from said check point, this is known as Synchronization. Authentication, logon/logoff and media streaming occur at this level.

**Layer 4: Transport**  
Transport protocols may be connection-oriented (TCP) or connectionless (UDP). Data is split into segments (size is determined by Layer 3), with a header detailing; origin port, destination port and sequence number. If there is an error in data transmission Layer 4 will trigger the Session Layer to resend data from the relevant check point, thus flow control and error handling occur at this level. Layer 4 is essentially concerned with service-to-service transmission of data via ports.

**Layer 3: Network**   
The connection between different devices is handled at this layer. Another header is added with the origin IP address, destination IP address and other metadata. Routing, the best path to transmit packets, is a component of this layer. Layer 3 is essentially concerned with end-to-end (client-server) transmission of data via logical/IP addresses.

**Layer 2: Data Link**  
The Data Lik Layer is split into 2 sub layers;

1. Logical Link Control Layer - Handles framing, flow control and elements of error control.
2. Media Access Control Layer - Handles congestion control and places frames on the Physical Layer for transmission as well as removing them.

The Data Link layer performs the following functions;

1. Framing - Packets from the Network Layer are divided into frames with a header specifying the origin and destination physical MAC address, a Frame Sequence Number for flow control and other data. A trailer is also added which contains error detection bits.
2. Flow Control - Ordering of frames as well as transmission speed and amount of data is managed so that the receiver is not overwhelmed with data leading to the loss of frames. Buffers can be used at either end to aid control flow.
3. Error Control - Errors in the LLC Layer are detected via Cyclic Redundancy Checks and are passed to Layer 4 so that data can be resent. Additionally the MAC layer can correct certain errors that arise in the Physical Layer.
4. Congestion Control - Multiple devices may try to transmit data at the same time accross media. The MAC layer controls access to this media so that data collisions are prevented and no data loss occurs. Media here refers to the communication medium between 2 nodes such as an ethernet cable, fibre optic or WiFi band.

Layer 2 connections are local, occuring within a network and not between networks. Layer 2 is essentially concerned with node-to-node (hop-by-hop) transmission of data via physical/MAC addresses.

**Layer 1: Physical**  
The Physical Layer is responsible for the transmission of unstructured raw data between physical devices (such as NIC cards, network switches etc) over a transmission medium. It converts bits into electrical, radio or optical signals.

**OSI Communication Flow**  
Two OSI compatible systems communicating over a network proceeds as follows;

1. Data at the topmost layer of the transmitting device (layer N) is composed into a protocol data unit (PDU).
2. The PDU is passed to layer N−1, where it is known as the service data unit (SDU).
3. At layer N−1 the SDU is concatenated with a header, a footer, or both, producing a layer N−1 PDU. It is then passed to layer N−2.
4. The process continues until reaching the lowermost level, from which the data is transmitted to the receiving device.
5. At the receiving device the process is reversed with the data passing from the lowest to the highest layer.

## TCP/IP Model

Like the OSI mode TCP/IP (Internet Protocol Suite) is designed to standardise computer networking. The OSI model is widely referenced and useful when thinking about networking but it is seldom implemented in full in modern computer networking. TCP/IP on the other hand is standard in most modern computer systems.

**TCP/IP Model Layers as specified in RFC1122**
<ol reversed>
 <li>Application Layer</li>
 <li>Transport Layer</li>
 <li>Internet Layer</li>
 <li>Link Layer</li>
</ol>

Note: Cisco certification splits the Network Acces Layer into Data Link and Physical Layers leading to a 5 layer TCP/IP model.

**TCP/IP Layer Architecture**

**Layer Architecture**
 <table>
  <tr>
    <th colspan=2>Layer</th>
    <th>PDU</th>
    <th>Function</th>
    <th>Protocols</th>
  </tr>
  <tr>
    <td>4</td>
    <td>Application</td>
    <td>Data</td>
    <td>Application level communication services split into user protocols (e.g. HTTP, FTP) and support protocols (e.g. DNS)</td>
    <td>HTTP, FTP, DNS</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Transport</td>
    <td>Segment or UDP Datagram</td>
    <td>End-to-end host communication that may be connection-oriented or connectionless</td>
    <td>TCP, UDP</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Internet</td>
    <td>Packet</td>
    <td>Routing using heirarchical IP addressing</td>
    <td>IPv4,IPv6</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Link</td>
    <td>Frame</td>
    <td>Communication of data between hosts on the same link</td>
    <td>PPP, Ethernet</td>
  </tr>
</table> 

**Application Layer**  
This layer is analogous to layer 5, 6 and 7 of the OSI Model, and provides most of the same services and protocols. RFC1122 categorises protocols as;  
* User Protocols - Such as HTTP, that provide services directly to the user.
* Support Protocols - That provide common system functions (name mapping, booting etc) such as DNS.

**Transport Layer**  
The transport layer provides end-to-end communication services for applications. There are two primary transport protocols;  
* Transmission Control Protocol (TCP) - Slow, reliable and connection-oriented.  
* User Datagram Protocol (UDP) - Fast, unreliable and connectionless.

**Internet Layer**  
Uses the IP Protocol to route data and find the best path from origin to destination. The IP Protocol is connectionless and there is no guarantee that data will be transmitted reliably, it may arrive out of order, damaged, or not at all. The layers above IP are responsible for reliable data transmission.

The IP Protocol specifies a Maximum Transmission Unit (MUT) which determines the size of the packet. Layers above have to modify the size of their datagrams to comply with the MTU.

**Link Layer**  
A "link" refers to all hosts on a local network that are reachable without traversing a router. It is analogous to OSI Layer 2 and performs most of the same functions. Also known as Network AccessLayer but in the RFC1122 spec it is called Link Layer.

