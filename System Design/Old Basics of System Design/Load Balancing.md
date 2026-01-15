#system-design 
Ref: https://dev.to/karanpratapsingh/system-design-load-balancing-1k41

#load-balancers #load-balancing-algorithms #reverse-proxy #forward-proxy 

Load balancing lets us distribute incoming network traffic across multiple resources ensuring high availability and reliability by sending requests only to resources that are online. 
Are key component for horizontal scaling !!

> [!NOTE] 
> Load balancers can be used to mitigate DDoS Attacks

#### Load balancing Algorithms:
- Round robin
- Weighted round robin :- assign weights to certain servers; based on capacity
- Least connections
- Least response time
- Least bandwidth :- sends client requests to the server with the least traffic

#### Workload Distributions:
- **Host-based**: Distributes requests based on the requested hostname.
- **Path-based**: Using the entire URL to distribute requests as opposed to just the hostname.
- **Content-based**: Inspects the message content of a request. This allows distribution based on content such as the value of a parameter.
- **Geo-location-based**: Distributes the requests based on geographical location of the incoming request

#### L4 and L7 Load Balancers 

L4 load balancers use algorithms to calculate the best server based on the fewest connections and fastest response times and hence operate on layer 4 of OSI Model 

L7 load balancers can inspect and manipulate application-layer data, such as HTTP headers, URLs and hence operate on layer 7 of OSI Model
#### Benefits:
- Scalability
- Redundancy
- Flexibility
- Efficiency

The load balancer itself can be a single point of failure. To overcome this, a second or N number of load balancers can be used in a cluster mode.

And, if there's a failure detection and the active load balancer fails, another passive load balancer can take over which will make our system more fault-tolerant.

![[Pasted image 20241030094142.png]]

### Reverse Proxy and Load Balancers

Reverse proxy servers and load balancers are both components in a client-server computing architecture and both act as intermediaries in the communication between the clients and servers

A load balancer distributes incoming client requests among a group of servers to ensure satisfactory speed and optimized functioning

Whereas, a reverse proxy is a dummy server that just passes the request received from the client to server. It can be used even when you have 1 server or more. But the use of reverse proxy is to provide security. It acts as the "public face" of the server so the client directs the traffic to proxy server which in turn sends request to the actual server - thereby hiding details of the actual server. 
When the server produces the response, it sends it to proxy server which in turn adds it own host address details and sends it back to the client

##### Note 
A reverse proxy is a proxy for the server
A forward proxy is a proxy for the client 