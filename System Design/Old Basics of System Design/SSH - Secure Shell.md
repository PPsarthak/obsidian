#system-design #ssh 

Ref: https://www.youtube.com/watch?v=rlMfRa7vfO8 

**SSH (Secure Shell)** is a protocol that enables secure communication between two networked devices

Today SSH-2 is widely used to access remote server, file transfer and to establish a secure connection between client and server. It is regulated by Internet Engineering Task Force (IETF)

### SSH-1 vs SSH-2

![[SSH-2 vs SSH-1.png|650]]

### Steps involved in client-server communication via SSH-2

1. Establish a TCP connection, typically over port 22
2. Version negotiation (ID string exchange) - ensure that both parties are speaking the same language
3. Algorithm negotiation - client and server decide which algorithm will be used for encryption, key exchange and integrity check
4. Perform key exchange - using Diffie Hellman Key exchange







