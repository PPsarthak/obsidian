---
status: Done
last-reviewed: 2025-09-20
tags:
  - spring-boot
  - graceful-shutdown
---
#spring-boot #graceful-shutdown
### Graceful Shutdown

- Graceful shutdown is *enabled by default* with all four embedded web servers (Jetty, Reactor Netty, Tomcat, and Undertow) and with both reactive and servlet-based web applications. 
- It occurs as part of closing the application context and is performed in the earliest phase of stopping
- This stop processing uses a timeout which provides a grace period during which existing requests will be allowed to complete but no new requests will be permitted.
- Once shutdown has been initiated server will reject any new connections.
- Requests on existing idle connections will also be rejected.
- <mark style="background: #33EE99;">The exact way in which new requests are not permitted varies depending on the web server that is being used. </mark>
- Implementations may stop accepting requests at the network layer, or they may return a response with a specific HTTP status code or HTTP header. 
- E.g:
	- **Jetty, Reactor Netty, and Tomcat will stop accepting new requests at the network layer.** 
	- Undertow will accept new connections but respond immediately with a service unavailable (503) response.

To configure the timeout period:
```properties
spring.lifecycle.timeout-per-shutdown-phase=20s
```

 **Disabling graceful shutdown:**
```properties
server.shutdown=immediate
```
