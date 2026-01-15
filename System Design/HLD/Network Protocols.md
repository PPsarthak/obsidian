---
tags:
  - system-design-v2
  - high-level-design
last-reviewed: 2025-12-28
---
#system-design-v2 #high-level-design


---
# OSI Model (Open Systems Interconnection)

The **OSI Model (Open Systems Interconnection Model)** is a conceptual framework used to understand and design computer network architectures. It was developed by the **International Organization for Standardization (ISO)** to standardize communication between heterogeneous systems. The OSI model does not define a specific protocol; instead, it provides a **layered abstraction** that explains *how data flows from one computer to another over a network*.

The model consists of **seven layers**, each with a well-defined responsibility. By separating networking concerns into layers, the OSI model improves **interoperability**, **troubleshooting**, **scalability**, and **learning clarity**.

---

Before the OSI model, vendors built proprietary networking solutions that could not interoperate. This caused serious compatibility issues. The OSI model addressed this by:

* Providing a **common language** for networking
* Allowing **vendor-independent implementations**
* Making network design **modular and maintainable**
* Enabling easier **fault isolation and debugging**

Each layer communicates only with the layers directly above and below it, enforcing separation of concerns.
### Overview of the Seven Layers

| Layer Number | Layer Name   | Key Responsibility           |
| ------------ | ------------ | ---------------------------- |
| 7            | Application  | User-facing network services |
| 6            | Presentation | Data formatting, encryption  |
| 5            | Session      | Session management           |
| 4            | Transport    | End-to-end communication     |
| 3            | Network      | Logical addressing & routing |
| 2            | Data Link    | Framing & MAC addressing     |
| 1            | Physical     | Bit transmission over medium |

Mnemonic (Top to Bottom): **All People Seem To Need Data Processing**
Mnemonic (Bottom to Top): **Please Do Not Throw Sausage Pizza Away**
#### Layer 7 – Application Layer
##### Purpose
The **Application Layer** provides network services directly to end users or applications. It is the closest layer to the user and interacts with software such as browsers, email clients, and APIs.
##### Key Responsibilities
* User authentication
* Resource sharing
* File transfers
* Email services
* Network service availability
##### Common Protocols
* HTTP / HTTPS
* FTP / SFTP
* SMTP / POP3 / IMAP
* DNS
* SNMP

> [!note] Important Clarification
> The application layer **does not include the UI itself**. Instead, it provides networking capabilities used by applications.
> Example: When a browser requests a webpage, the HTTP request originates at the application layer.

#### Layer 6 – Presentation Layer
##### Purpose
The **Presentation Layer** ensures that data is in a usable and readable format before being passed to the application layer. It acts as a **translator** between different data formats.
##### Key Responsibilities
* Data encoding and decoding
* Data compression
* Encryption and decryption
##### Common Formats & Standards
* JSON, XML
* JPEG, PNG
* ASCII, Unicode
* TLS / SSL encryption

Example: If one system uses UTF-8 and another uses ASCII, the presentation layer handles conversion. HTTPS encryption also operates conceptually at this layer.

#### Layer 5 – Session Layer
##### Purpose
The **Session Layer** manages sessions between two communicating systems. A session is a logical connection that persists across multiple data exchanges.
##### Key Responsibilities
* Session establishment
* Session maintenance
* Session termination
* Synchronization and checkpoints
##### Example Use Cases
* Video streaming sessions
* Login sessions
* Remote procedure calls

> [!note] Real-World Mapping
> In modern systems, session handling is often integrated into application or transport layers, but conceptually it belongs here.

#### Layer 4 – Transport Layer
##### Purpose
The **Transport Layer** provides reliable or unreliable **end-to-end communication** between hosts. It ensures that data is delivered correctly, in order, and without duplication (if reliability is required).
##### Key Responsibilities
* Segmentation and reassembly
* Flow control
* Error control
* Port addressing
#### TCP vs UDP

| Feature             | TCP       | UDP            |
| ------------------- | --------- | -------------- |
| Reliability         | Yes       | No             |
| Connection-oriented | Yes       | No             |
| Speed               | Slower    | Faster         |
| Use Cases           | HTTP, FTP | DNS, Streaming |

Example: Microservices communicating via REST APIs rely heavily on TCP at this layer.
#### Layer 3 – Network Layer
##### Purpose
The **Network Layer** handles **logical addressing** and **routing** of data packets across different networks.
##### Key Responsibilities
* IP addressing
* Packet routing
* Path selection
* Fragmentation
##### Common Protocols
* IP (IPv4, IPv6)
* ICMP
* IPsec
* Routing protocols (OSPF, BGP)
* Devices Operating Here: Routers, Layer 3 switches

Example: When a packet travels from India to the US, routers use IP addresses to determine the optimal path.
#### Layer 2 – Data Link Layer
##### Purpose
The **Data Link Layer** ensures reliable data transfer between **directly connected nodes**.
##### Key Responsibilities
* Framing
* MAC addressing
* Error detection (CRC)
* Media access control
##### Sublayers
* **LLC (Logical Link Control)**
* **MAC (Media Access Control)**
##### Common Technologies
* Ethernet
* Wi-Fi (802.11)
* ARP
##### Devices Operating Here
* Switches
* Network Interface Cards (NICs)

Example: A switch forwards frames using MAC addresses at this layer.
#### Layer 1 – Physical Layer
##### Purpose
The **Physical Layer** is responsible for transmitting raw bits (0s and 1s) over a physical medium.
##### Key Responsibilities
* Voltage levels
* Bit rate control
* Cable standards
* Signal modulation
##### Examples
* Ethernet cables
* Fiber optics
* Radio waves
* USB
* Devices Operating Here: Hubs,  Repeaters,  Cables and connectors

### Data Flow Through the OSI Model

**Sending Side (Encapsulation)**
1. Application creates data
2. Presentation formats/encrypts data
3. Session establishes session
4. Transport segments data and adds ports
5. Network adds IP addresses
6. Data Link adds MAC addresses
7. Physical sends bits

**Receiving Side (Decapsulation)**: The process is reversed layer by layer.

---
### OSI Model vs TCP/IP Model

| OSI Model    | TCP/IP Model |
| ------------ | ------------ |
| 7 Layers     | 4 Layers     |
| Conceptual   | Practical    |
| ISO Standard | DARPA Model  |
| Detailed     | Simplified   |
TCP/IP combines several OSI layers into fewer layers but follows the same principles.

**Advantages of the OSI Model**
* Clear separation of concerns
* Easier troubleshooting
* Vendor interoperability
* Better learning and documentation
* Scalable architecture

**Limitations of the OSI Model**
* Rarely implemented strictly
* Some layers overlap in practice
* Slightly complex for small systems

#### Conclusion

The OSI Model is a **foundational concept in computer networking**. While modern networks rely primarily on the TCP/IP stack, the OSI model remains invaluable for understanding how networks work internally. It provides a clean mental model that helps engineers design, debug, and scale complex distributed systems.

For backend engineers, cloud architects, and system designers, mastering the OSI model is essential—not because it is implemented directly, but because it explains *why networking works the way it does*.


