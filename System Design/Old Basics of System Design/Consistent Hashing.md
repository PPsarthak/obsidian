#system-design #consistent-hashing #load-balancers #content-delivery-networks 
Load balancer's primary objective is to distribute the incoming requests equally amongst the servers. The simplest way to achieve this is using hashing. 

But what can be the constraints here?
![[Consistent Hashing 1.png|600]]
Sometimes it may happen that a request (say on server 1) is large and causes a load on the server. Hence, it may be optimal to send the next request on that server to another server. Definitely a constraint !

But let's keep this aside for a minute and assume that all requests put same load on the server. Now what are the constraints ?

What happens if server 1 fails, now obviously our hash function will still assume that no of servers = 4 and keep sending request to server 1. So, the obvious solution is to change the hash function .....hmm a problem for sure right?
So now, you have changed the hash function's N to 3. Now what? The hashing has been disturbed - since N has changed, the hash function *might* not distribute the requests equally...leading to ***inconsistent hashing***

![[Consistent Hashing 2.png|600]]

#### Consistent Hashing Algorithm
> In addition to hashing request/object keys we also hash the servers with the same hash function and same range N

Let x0 to xN represent the hash range i.e., the output of hash function lies between these values. As part of this algorithm, we create a circular ring of this range.
Initially, we hash our servers and place them on the ring 

![[Consistent Hashing 3.png|600]]

Now when requests arrive, they are hashed and placed on this ring and are redirected to immediate next server when moving clockwise

![[Consistent Hashing 4.png|500]]

#### Case 1: New Server is added

In this case, only k0 needs to be moved (i.e., requests that come between s3 and s0 needs to be moved)
![[Consistent Hashing 5.png|500]]

#### Case 2: Existing Server is removed
Same conclusion, only requests between s4 and s3 needs to be moved
![[Consistent Hashing 6.png|600]]

## Potential Issues

All the available servers should be equally distributed across the hash function - which is very less likely. Chances are they might not get equally distributed

![[Consistent Hashing 7.png|550]]

### Simple solution
Have multiple virtual nodes for 1 real server - 

![[Consistent Hashing 8.png|550]]

In real life, there are much more virtual nodes and obviously no.of virtual nodes ∝ equal distribution 

### Use cases

| Where?             | Why?                             |
| ------------------ | -------------------------------- |
| Databases          | Data partitioning                |
| CDN                | Distribute web content evenly    |
| [[Load Balancing]] | Distribute persistent connection |
