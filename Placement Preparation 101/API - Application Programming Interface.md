---
tags:
  - placement-preparation
  - api
last-reviewed: 2026-01-17
---

At its core, an API (Application Programming Interface) is a contract that allows two software systems to talk to each other in a controlled and predictable way.

Think of an API as a middle layer (of abstraction) 
One system exposes certain functionality (data or actions), and another system consumes it without knowing the internal implementation. The consumer only needs to know what is available, how to request it, and what response to expect.

### Popular REST API Styles
1. REST
2. Soap
3. GraphQL
4. gRPC
5. Webhooks
6. WebSockets

---
#### Representational State Transfer
- REST is the most widely used API style on the web.
- It is **resource-based**, meaning everything is treated as a resource (users, orders, products) identified by URLs. Or more professionally, we say that resources are identified by URIs (Uniform Resource Identifier)
- REST **uses standard HTTP methods** like `GET`, `POST`, `PUT`, `PATCH`, and `DELETE` to perform operations. 
- It is stateless, so each request MUST contain all the information needed to process it.
- REST APIs typically *exchange data in JSON format and leverage HTTP status codes for communication.*

---

#### Simple Object Access Protocol
- SOAP is much stricter than REST. 
- It **uses XML for message formatting** and follows a well-defined contract called WSDL (Web Services Description Language).
- SOAP supports advanced features like built-in security (WS-Security), transactions, and reliable messaging.

> While highly standardized and secure, SOAP is verbose, harder to debug, and slower compared to modern alternatives. Today, it’s less common in new systems but still important in enterprise integration scenarios.

---
#### GraphQL
- GraphQL is a *query-based API style where the client specifies exactly what data it needs*. 
- Instead of multiple endpoints, **GraphQL exposes a single endpoint** and uses a strongly typed schema. This avoids over-fetching and under-fetching, a common REST problem.
- Clients can fetch deeply nested and related data in a single request, which improves performance for complex UIs. 
- However, GraphQL introduces complexity on the server side, including schema management and query validation. 
- Caching is also harder compared to REST. 
- It’s commonly used in frontend-heavy applications where flexibility and efficiency are critical.

---

#### gRPC
- gRPC is a high-performance RPC framework 
- It uses Protobuf for compact, binary data serialization, making it much faster than JSON-based APIs.
- gRPC **supports bi-directional streaming and works efficiently over HTTP/2**.
- It’s commonly used for microservice-to-microservice communication, especially in internal systems where performance matters. 
- However, *gRPC is not browser-friendly by default* and is harder to debug due to binary payloads.

---

#### Webhooks
- Webhooks are **event-driven APIs where a server pushes data to a client when something happens**, instead of the client polling repeatedly.
- *You register a callback URL, and when an event occurs (e.g., payment success), the server sends an HTTP request to your endpoint.*
- Webhooks are simple, efficient, and ideal for real-time notifications. However, they require your server to be publicly accessible and robust enough to handle retries and duplicate events.
- They are widely used in payment gateways, CI/CD tools, and SaaS integrations.

---

#### WebSockets
- WebSockets **provide full-duplex, persistent communication between client and server over a single connection.** 
- Unlike REST, which is request-response based, **WebSockets allow real-time data flow in both directions.**
- WebSockets are more complex to scale, monitor, and secure compared to REST APIs. They’re best used when real-time interaction is a core requirement.
- They are ideal for use cases like chat applications, live dashboards, gaming, and stock price updates.

---
### HTTP Status Codes

![[HTTP Status Codes.png]]

---
### API Versioning
API versioning is the practice of managing changes to an API without breaking existing clients. 
Since APIs are contracts, any incompatible change must be handled carefully—versioning is how you evolve safely

**Versioning lets multiple API behaviors coexist.**

---
##### Common API Versioning Strategies
###### 1. URI Versioning (Most Common)
- Example:
- /api/v1/users → /api/v2/users
- ✔ Simple and visible
- ✘ Pollutes URLs, but widely accepted and interview-friendly

###### 2. Header Versioning
- Example:
- Accept: application/vnd.myapp.v2+json
- ✔ Clean URLs
- ✘ Harder to debug, less obvious

###### 3. Query Parameter Versioning
- Example: `/users?version=2`
- ✔ Easy to implement
- ✘ Generally discouraged for public APIs

###### 4. Content Negotiation
 - Uses media types to decide version. Powerful but complex and rarely worth it unless you have strict standards.


### Advanced topics to cover
1. API Design & Modeling
	- Resource modeling vs action-based APIs
	- URI design best practices
	- Request/response schema design
	- Versioning strategies (URL, header, media-type)
	- Idempotency and safe operations

2. API Security
	- Authentication vs Authorization
	- API keys, OAuth 2.0, OpenID Connect
	- JWT structure and pitfalls
	- Rate limiting and throttling
	- Securing public vs internal APIs

3. Performance & Scalability
	- Pagination, filtering, sorting
	- Caching strategies (HTTP cache headers, CDN, Redis)
	- N+1 problem in APIs
	- Bulk APIs vs chatty APIs

4. Reliability & Resilience
	- Timeouts, retries, and exponential backoff
	- Circuit breakers and fallbacks
	- Idempotent retries
	- Handling partial failures

5. API Documentation & Contracts
	- OpenAPI / Swagger
	- Contract-first vs code-first APIs
	- Backward compatibility guarantees

6. Observability & Governance
	- Logging, metrics, tracing
	- API gateways and policies
	- Deprecation and lifecycle management

7. Advanced Communication Patterns
	- Sync vs async APIs
	- Event-driven APIs
	- Webhooks reliability patterns
	- Streaming APIs