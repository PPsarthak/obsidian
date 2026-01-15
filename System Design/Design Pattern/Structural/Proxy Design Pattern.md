#system-design-v2 #design-patterns #structural-design-pattern 

## Glossary

- Structural design pattern where a surrogate or placeholder object controls access to another object (real subject). 
- The proxy exposes the same interface as the real object, so clients can interact with it transparently.

> <mark style="background: #FF6699;">Think of Proxy as a gatekeeper</mark>
> At a high level, Proxy answers the question: <mark style="background: #33EE99;">Do you want to access this object directly, or should something stand in between?</mark>
##### Terminologies
**Subject**: A common interface for both the real object and the proxy.
**Real Subject**: The actual object that performs the core business logic.
**Proxy**: An object that implements the same interface and forwards requests to the real subject, optionally adding behavior.

The proxy may additional behavior:
- Delay object creation
- Check permissions
- Cache results
- Log access
- Manage remote communication

But still unlike Decorator Pattern (which also adds additional behavior), proxy adds behavior for access conditions

#### Key Traits
- Same interface as real object → transparency
- Lazy interaction → real object may not be created immediately
- Access control → pre/post checks
- Single point of interception → centralized logic

### Typical Use Cases
- Accessing remote services (RPC, REST clients)
- Security checks before method invocation
- Expensive resource initialization
- API rate limiting and throttling
- ORM lazy loading (e.g., **Hibernate proxies**)

#### How it works - @Transactional
`@Transactional` uses proxy internally, so 
- When you start a transaction, a proxy is created
- If all the operations are successful, the proxy calls the real object and commits them
- If anything fails, the proxy is destroyed and no real data is committed

![[UML Class Diagram for Proxy Design Pattern.png|450]]