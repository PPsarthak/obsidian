---
status: Done
last-reviewed: 2025-09-07
tags:
  - spring-boot
  - eureka-server
---
#spring-boot #eureka-server
### Outline
- Eureka Server Dependency & Setup - [[#Eureka Server Dependency & Setup]]
- Eureka Client Dependency & Setup - [[#Eureka Client Dependency & Setup]]
- Concept of Service Registry - [[#Concept of Service Registry]]
- What is Eureka Server? - [[#What is Eureka Server?]]
- Service Registration Process - [[#Service Registration Process]]
- Decoupling services for scalability and maintainability - [[#Decoupling Services for Scalability & Maintainability]]
- Dynamic service registration and discovery - [[#Dynamic Service Registration and Discovery]]
- Load balancing with Eureka + Ribbon/Spring Cloud Load Balancer - [[#Load Balancing with Eureka + Ribbon/Spring Cloud Load Balancer]]
- Fault tolerance & high availability in distributed systems - [[#Fault Tolerance & High Availability in Distributed Systems]]

---
### Eureka Server Dependency & Setup

1. Add dependency in pom
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

2. Enable Eureka Server in the application
```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

3. Add properties
```properties
# Application name
spring.application.name=eureka-server

# Port
server.port=8761

# Since this is a server, it should not register itself
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

4. Access the application dashboard - `http://localhost:8761`
### Eureka Client Dependency & Setup

1. Add Eureka Client Dependency
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2. Enable Eureka Client
```java
@SpringBootApplication
@EnableEurekaClient   // optional in newer versions
public class PaymentServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(PaymentServiceApplication.class, args);
    }
}
```

3. Configure properties
```properties
# Application name - how it will appear in Eureka dashboard
spring.application.name=payment-service
server.port=8081

# Eureka server location (where this client should register)
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

4. Run server and then client
	- Start Eureka Server (http://localhost:8761)
	- Start the client service → it automatically registers itself
---
### Concept of Service Registry
- In a microservices architecture, applications are broken down into many independent services that often run on dynamic environments like containers or cloud instances. These services typically do not have fixed network addresses because they may scale up or down, restart, or move across machines. 
- A Service Registry acts as a centralized database that keeps track of all available services and their instances, along with their host and port information. Instead of hardcoding IP addresses, services register themselves with the registry at startup and deregister on shutdown. 
- Other services can then query the registry to discover where a particular service is running at any given moment. This makes the system more flexible, fault-tolerant, and scalable because services don’t need to know the exact network location of one another; they only need to know how to ask the registry.
### What is Eureka Server?
- **Part of Netflix OSS** → Eureka is a service registry component provided by Netflix and integrated with Spring Cloud.
- **Acts as a Service Registry** → Keeps track of all running service instances (name, host, port, metadata).
- **Self-registration** → Microservices automatically register themselves with Eureka at startup.
- **Service discovery** → Other services query Eureka to dynamically find available services instead of relying on static IPs.
- **Health checks** → Eureka regularly receives heartbeat signals from services to confirm they are alive; if a service stops sending, Eureka removes it from the registry.
- **Decentralized & resilient** → Can run in standalone mode or in a cluster for high availability.
- **UI dashboard** → Provides a web-based interface at / (default port 8761) to visualize registered services and their statuses.
### Service Registration Process
When a client starts:
- Sends a registration request to Eureka Server with metadata (host, port, health URL, app name).
- Continues sending heartbeat signals at regular intervals (default: 30s) to confirm it is alive.
If the client fails to send heartbeats, Eureka assumes it’s down and removes it from the registry.
### Use Cases
#### Decoupling Services for Scalability & Maintainability
In tightly coupled systems, services need to know exact IPs/ports of other services -hard to scale and update.  (Like imagine, hardcoding `localhost:9092` for any service)
How Eureka helps:
- Services interact using logical names (e.g., payment-service) instead of static addresses.
#### Dynamic Service Registration and Discovery
In microservices, instances often change due to scaling (new pods/containers spun up) or failures (old ones shutting down). Hardcoding IP addresses is brittle and unscalable. 
How Eureka helps:
- Services register themselves automatically at startup with Eureka, providing their host, port, and metadata.
- Eureka keeps track of this registry dynamically.
- Other services don’t need to know where a service is running — they just query Eureka by the service’s name.

#### Load Balancing with Eureka + Ribbon/Spring Cloud Load Balancer
When multiple instances of the same service are running, requests should be distributed evenly instead of always hitting a single instance.
How Eureka helps:
- Eureka provides a list of all active instances for a service.
- Ribbon (legacy) or Spring Cloud Load Balancer (modern alternative) uses that list to balance requests across instances.

#### Fault Tolerance & High Availability in Distributed Systems
Services can fail unexpectedly due to crashes, network issues, or scaling operations
How Eureka helps:
- Monitors service health via periodic heartbeat pings from registered services.
- If a service fails to respond within a time window, Eureka removes it from the registry.
- Clients automatically get updated lists of healthy instances.

### **Quick Recap / Key Takeaways**
Service Registry
- Centralized database for tracking services and their network locations.
- Eliminates hardcoded IPs → services register themselves and discover others dynamically.

Eureka Server
- Netflix OSS component integrated with Spring Cloud.
- Acts as a service registry with UI dashboard (default: http://localhost:8761).
- Supports self-registration, service discovery, and health checks.
- Can run standalone or in clusters for high availability.

Eureka Client
- Any microservice that registers with Eureka Server and uses it for service discovery is Eureka Client.
- On startup, client registers with Eureka Server (app name, host, port, metadata).
- Sends periodic heartbeats (default: every 30s) to confirm it’s alive. If heartbeats stop → Eureka evicts the client.
- Removes the need for hardcoding IPs → services use logical names instead.
- When calling another service, the client queries Eureka Server by service name.
- Gets a list of healthy instances → forwards request to one (often via a load balancer).

Key Use Cases
1. Decoupling & Scalability → Services communicate loosely via registry → easier scaling, maintenance, and deployments.
2. Dynamic Service Registration & Discovery → Services find each other by logical names, not IPs.
3. Load Balancing → Works with Ribbon (legacy) or Spring Cloud Load Balancer to spread requests across instances.
4. Fault Tolerance → Removes dead instances from registry, ensuring requests only go to healthy services.
5. Simplified Communication → Clients talk using service names, not IPs