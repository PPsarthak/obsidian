#computer-networks #placement-preparation 

### Basic Terminologies of Computer Networks
**Computer Network** : A network of connected computers that facilitate exchange of data

**Nodes** : Nodes are devices that are connected to a network - computers, servers, printers, routers, switches, and other devices.

**Topology** : The physical and logical arrangement of nodes

**Protocol** : A protocol is a set of rules and standards that govern how data is transmitted over a network.

**IP Address** : unique numerical identifier that is assigned to every device on a network.

**DNS** :  protocol that is used to translate human-readable domain names (such as www.google.com) into IP addresses that computers can understand (which is 8.8.8.8). #dns 

**Firewall** : security device that is used to monitor and control incoming and outgoing network traffic. Firewalls are used to protect networks from unauthorized access and other security threats.

**Modem**: used to access internet by customers of an Internet Service Provider.
**Repeater** : used to amplify the received signal and transmit it again over long distances (*physical layer*)
**Hub** : broadcasts the received data to all ports/devices (it is like a multi-port repeater) (used in star topology) (*physical layer*)
**Bridge** : used to connect 2 LANs together or maybe divided 1 LAN into 2 LANs for better management (*data link layer*)
**Switch** : intelligent-hub, sends the received data only to desired location's MAC Address and also perform error checking (*data link layer, some modern ones are at network layer*)
**Router** : routes data based on IP Address (so it is obviously *network layer*)
**Gateway** : like a bridge that connects 2 networks but with different networking models

---
##### Network topology 
#network-topology 

![[Types of Network Topology.png|750]]

###### Types of Area Networks
#area-networks
*LAN* covers the smallest area, *MAN* covers an area larger than LAN and *WAN* comprises the largest of all. 

##### Unique Identifiers of a Network
**Hostname** : each device in network has a hostname, which can be seen using the command - `hostname`
**MAC Address** : (*physical address*) unique id associated with NIC (Network Interface Card), command - `ipconfig/all` (48 bytes)
**IP Address** : (*logical address*) unique id assigned to each device on a network , command - `ipconfig` (32 bits or 128 bits)
**Socket** : Combination of IP Address and Port
**Port** : logical channel to send and receive data, command - `netstat -a` (16 bit port number)

| Port Types       | Range       |
| ---------------- | ----------- |
| Well known ports | 0-1023      |
| Registered       | 1024-49151  |
| Ephemeral        | 49152-65535 |

---
#### Network Protocols
Network protocols define how 2 devices in a network behave and communicate

###### TCP
#tcp 
![[TCP Protocol.png|550]]
transport layer protocol that establishes a connection between devices before sending the data *transport layer*
###### IP
#ip 
responsible for addressing and routing packets of data between devices; uses IP addresses to identify source and destination devices *transport layer*
###### UDP
#udp  
![[UDP Protocol.png|550]]
connectionless and reliable protocol making it faster but less secure than TCP *transport layer*
###### HTTP
#http 
Request-response protocol that is stateless and allows to render HTML docs, images, etc. *application layer*
###### DNS
#dns [[Domain Name System]]
translates human-readable domain names to IP addresses *application layer*
###### TLS
#tls
transport layer security ensures secure communication between client-server by providing encryption, authentication and data integrity
###### DHCP
#dhcp
simplifies IP address management by dynamically assigning IP address
###### SSH
#ssh [[SSH - Secure Shell]]
secure remote access to devices over a unsecured network
###### ICMP
#icmp
Internet control message protocol is used to diagnose network issues and errors

---
#### OSI Model 
![[OSI Model]]
