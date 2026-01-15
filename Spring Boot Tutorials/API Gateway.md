---
status: Done
last-reviewed: 2025-09-07
tags:
  - "#spring-boot"
  - api-gateway
---
#spring-boot #api-gateway 

### Outline
- Dependency & Setup - [[#Dependency & Setup]]
- Concept of an API Gateway - [[#Concept of API Gateway]]
- Centralized routing and service discovery integration - [[#Centralized Routing & Service Discovery Integration]]
- Security and authentication - [[#Security & Authentication]]
- Cross-cutting concerns (logging, rate limiting, monitoring) - [[#Cross-Cutting Concerns (Logging, Rate Limiting, Monitoring)]]
- Protocol translation and request/response manipulation - [[#Protocol Translation & Request/Response Manipulation]]

---

### Dependency & Setup
1. Add Dependency
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

2. Configure properties
```properties
spring.application.name=api-gateway
server.port=8080

# Example route to user-service
spring.cloud.gateway.routes[0].id=user-service
spring.cloud.gateway.routes[0].uri=http://localhost:8081
spring.cloud.gateway.routes[0].predicates[0]=Path=/users/**

# Example route to order-service
spring.cloud.gateway.routes[1].id=order-service
spring.cloud.gateway.routes[1].uri=http://localhost:8082
spring.cloud.gateway.routes[1].predicates[0]=Path=/orders/**
```

3. Enable Service Discovery with [[Eureka]]
- Instead of hardcoding URLs, API Gateway can use Eureka for dynamic service lookup
```properties
spring.cloud.gateway.discovery.locator.enabled=true
spring.cloud.gateway.discovery.locator.lower-case-service-id=true
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

4. Run the Gateway
- Start Eureka Server → register microservices → start API Gateway.
- Gateway will auto-discover services and act as the single entry point.

### Concept of API Gateway
- An API Gateway is a single entry point for all client requests in a microservices architecture. Instead of clients calling each service directly (which would require them to know each service’s location), they send all requests to the gateway.
- The gateway then forwards (or routes) those requests to the appropriate microservice. This decouples the client from the internal structure of the system, allowing microservices to scale, evolve, or change independently without affecting clients.
- Spring Cloud Gateway is the official API Gateway implementation provided by the Spring team, built on top of Spring Web Flux and Project Reactor.
- It supports both synchronous and reactive applications, and provides rich features like dynamic routing, filtering, load balancing, security, monitoring, and integration with service discovery (Eureka).
#### Key Features of Spring Cloud API Gateway
- **Single entry point** → All client traffic passes through the gateway.
- **Dynamic routing** → Routes requests to services based on path, headers, or other conditions.
- **Service discovery integration** → Works with Eureka, Consul, or other registries for dynamic routing.
- **Cross-cutting concerns** → Centralized logging, security, authentication, and monitoring.
- **Request/response filtering** → Modify headers, rewrite paths, block requests, or enrich responses.
- **Reactive support** → Built on Spring Web Flux for handling high concurrency with non-blocking I/O.
- **Protocol bridging** → Can translate between protocols (e.g., HTTP to WebSocket).

### Use Cases
#### Centralized Routing & Service Discovery Integration
Clients would need to know the address of every microservice
How Gateway helps:
- Acts as a single entry point for all client requests.
- Integrates with Eureka (or other service registries) to dynamically route traffic.
- Clients only need to talk to the gateway → microservices remain hidden and flexible.

#### Security & Authentication
Implementing authentication in every microservice leads to duplication and inconsistency
How Gateway helps:
- Centralized security enforcement (OAuth2, JWT, API keys, etc.).
- Requests can be authenticated/authorized at the gateway before reaching downstream services.
- Prevents unauthorized or malformed requests from even touching the services.

#### Cross-Cutting Concerns (Logging, Rate Limiting, Monitoring)
Adding features like logging, tracing, or rate limiting individually to every microservice increases complexity.
How Gateway helps:
- Provides filters to implement cross-cutting concerns at a single point.
- Examples:
	- Log incoming/outgoing requests.
	- Apply rate limiting to prevent abuse.
	- Add tracing headers for observability (Zipkin, Sleuth, Datadog).
- Consistency and reduced boilerplate across services.

#### Protocol Translation & Request/Response Manipulation
Different clients (web, mobile, IoT) may need different formats or protocols.
How Gateway helps:
- Request transformation → Rewrite URLs, modify headers, or enrich payloads before forwarding.
- Response transformation → Modify service responses before sending them back to the client.
- Protocol bridging → Convert HTTP to WebSocket (real-time communication).
Clients get responses in a unified format without services needing to know about client-specific requirements.

### **Quick Recap/Key Takeaways**

What is API Gateway?
- A single entry point for all clients in a microservices system.
- Built on Spring Web Flux (reactive & non-blocking).
- Handles routing, filtering, security, and cross-cutting concerns.

Use Cases
- Centralized routing → Clients don’t need to know service locations.
- Security/authentication → Enforce policies at a single place.
- Cross-cutting concerns → Logging, monitoring, rate limiting, tracing.
- Request/response manipulation → Rewrite paths, headers, protocols.