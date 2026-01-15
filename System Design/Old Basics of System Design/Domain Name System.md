#system-design #dns 
DNS translates human-readable domain names to machine readable IP addresses; so it is like a directory of IP addresses.

### How does DNS works?

![[Pasted image 20241110084905.png]]

Let's understand the process and each step associated with it
##### DNS Query
Whenever a browser receives a request it first resolves the DNS of the request by running a ***DNS query***. (Indicated by 1 in the above diagram)

##### DNS Resolver
The browser gives this DNS query to the ***DNS resolver*** (which first checks if it has the IP stored in its cache). It is also called as ***Recursive Resolver***
After receiving the DNS query, it sends a request to a ***root nameserver***, followed by another request to a ***TLD nameserver***, and then one last request to an ***authoritative nameserver.*** 
After receiving a response from the authoritative nameserver containing the requested IP address, the recursive resolver then sends a response to the client.

##### DNS Root Nameserver
A root nameserver responds by directing the recursive resolver to a TLD nameserver.
There are many TLD servers 

<span style="color:rgb(255, 49, 49)">!incomplete</span>
