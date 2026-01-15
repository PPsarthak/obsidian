---
date: 2025-09-27
tags:
  - aws
---
#aws

Amazon EC2 is hosted in multiple locations world-wide. These locations are composed of AWS Regions, Availability Zones, Local Zones, AWS Outposts, and Wavelength Zones.

Regions are separate geographic areas.
- **Availability Zones** are multiple, isolated locations within each Region.
- **Local Zones** provide you with the ability to place resources, such as compute and storage, in multiple locations closer to your end users.
- **Wavelength Zones** provide you with the ability to build applications that deliver ultra-low latencies to 5G devices and end users. Wavelength deploys standard AWS compute and storage services to the edge of telecommunication carriers' 5G networks.
- **AWS Outposts** brings native AWS services, infrastructure, and operating models to virtually any data center, colocation space, or on-premises facility.

AWS operates state-of-the-art, highly available data centers. Although rare, failures can occur that affect the availability of instances that are in the same location. If you host all of your instances in a single location that is affected by a failure, none of your instances would be available.

#### Regions
Think of AWS Region as a geographical area in the world where AWS has data centers.
When you are preparing to deploy a workload, consider which Region or Regions best meet your needs. 
Select a Region that has the AWS services and features that you need. Also, you can lower network latency when you select a Region that is close to the majority of your users.


> Inside a Region → Availability Zones (AZs)

Each AWS Region has multiple Availability Zones (AZs)

Each Region is designed to be isolated from the other Regions. This achieves the greatest possible fault tolerance and stability.

When you launch an instance, select a Region that puts your instances close to specific customers, or that meets the legal or other requirements that you have. You can launch instances in multiple Regions.

When you view your resources, you see only the resources that are tied to the Region that you specified. This is because Regions are isolated from each other, and we don't automatically replicate resources across Regions.