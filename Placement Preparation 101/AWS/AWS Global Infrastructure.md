#aws 

AWS Global Infrastructure refers to the layout of AWS regions and availability zones around the world. 
The AWS infrastructure is built with numerous Regions, each containing at least three Availability Zones that are far enough apart to avoid correlated failures but close enough for low-latency connectivity. This design enables high availability, fault tolerance, and scalability for applications deployed across multiple AZs within a Region.  
#### AWS Region
A region is a geographical area, where AWS maintains its data centers. Each consisting of two or more availability zones (AZs) which are engineered to be isolated from failures in other AZs.