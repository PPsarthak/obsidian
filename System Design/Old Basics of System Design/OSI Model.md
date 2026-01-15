#networking #tcp-ip #osi #http #system-design

Provides a way of thinking and dividing the Internet into layers ... abstraction layers

> Actual communication happens top to bottom not from bottom to top !!! 
> Which means, when 2 devices communicate, first an application layer adds a HTTP header, then comes presentation layer, then session layer and so on...finally physical layer which actually transmits the data

Physical Layer: (wires)
- Contains hardware components that are responsible for **transmitting bits of data**
- Which converts digital data into electrical, light, or radio signals based on the transmission medium (cable, fiber optic, wireless)

Data-link Layer: (Ethernet)
- Takes the raw bits of data and **convert it to frames**
- Additionally, adds MAC addresses to identify devices on a network

Network Layer: (IP)
- Responsible for **addressing and routing packets** of data to the correct destination 
- Assigns IP addresses to packets, determines the *optimal path* across networks, and manages packet forwarding.

Transport Layer: (TCP and UDP)
- Handles end to end communication and data delivery
- Meaning? It is obvious that the entire data cannot be transmitted, so it divides this into segments - assigning each of them with a segment number. Now when it reaches the destination, these segments are merged into one based on the segment number
- TCP also provides checksum to ensure that data is not corrupted during transmission

Session Layer:
- Managing and maintaining sessions (connections) between applications.

Presentation Layer:
- Data translation, encryption, and compression.

Application Layer: ([[HTTP]])
- Direct interaction with end-user applications - Provides network services (like HTTP for web browsing, FTP for file transfer) to applications for sending/receiving data.
- Acts as the user’s interface to the network, allowing applications to request and provide network services based on protocols.

---
##### Note:
The Session, Presentation and Application Layers of OSI Model are too fine grained. Hence they can be compressed into one - and that's what the TCP/IP Model does !

**Session Layer**: In practice, session management is often handled by the Transport layer (TCP), which establishes and maintains connections directly.

**Presentation Layer**: In modern applications, these tasks are typically integrated into the application itself or handled by libraries and middleware, not by a dedicated layer.

**Application Layer**: TCP/IP combines the OSI application, presentation, and session functionalities directly under this single layer.

---
##### Flow of communication between 2 devices
Here is the flow of how data will be transmitted from device A to device B

![[OSI Model Flow - ByteByteGo.png]]
_At each layer, the data is called with different name. At transport layer, it is segment, at network layer it is datagram, at data link layer it is frame, and finally at physical layer it is bits_

Now as seen in image, point 1 adds the HTTP header to the data. This is done by the application layer.
Next, up a TCP header is added to the data at transport layer.
The IP header is added at network layer and it contains the source and destination IP details.
The MAC header is added at data link layer.


### TCP and UDP
#tcp #udp
Transmission Control Protocol is a connection oriented protocol - i.e., once a connection is successfully established, data can be transmitted in both direction. 

![[TCP Protocol.png|850]]

User Datagram Protocol, is a simpler, connectionless internet protocol. With UDP, there is no overhead for opening a connection, maintaining a connection, or terminating a connection. Data is continuously sent to the recipient, whether or not they receive it.

![[UDP Protocol.png|850]]

#### TCP vs UDP
A key difference between TCP and UDP is speed, as TCP is comparatively slower than UDP. Overall, UDP is a much faster, simpler, and more efficient protocol, however, retransmission of lost data packets is only possible with TCP.

TCP provides ordered delivery of data from user to server (and vice versa), whereas UDP is not dedicated to end-to-end communications, nor does it check the readiness of the receiver.

|Feature|TCP|UDP|
|---|---|---|
|Connection|Requires an established connection|Connectionless protocol|
|Guaranteed delivery|Can guarantee delivery of data|Cannot guarantee delivery of data|
|Re-transmission|Re-transmission of lost packets is possible|No re-transmission of lost packets|
|Speed|Slower than UDP|Faster than TCP|
|Broadcasting|Does not support broadcasting|Supports broadcasting|
|Use cases|HTTPS, HTTP, SMTP, POP, FTP, etc|Video streaming, DNS, VoIP, etc|

![[TCP and UDP Header Format.png]]

