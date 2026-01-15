#system-design-v2 #design-patterns #behavioral-design-pattern 

## Glossary
- The Chain of Responsibility pattern passes requests along a chain of handlers. Each handler either handles it or passes it to next handler
- This allows for flexible and dynamic handling of requests without the client needing to know the specific receiver. 
- Every processor in the chain should have reference to the next processor
#### How it works
- **Decoupling**: The sender of a request is not directly connected to a specific receiver. Instead, it sends the request to the first object in the chain.
- **Sequential processing**: The request moves from one handler to the next in the chain. Each handler is responsible for a specific type of request or a certain level of authority.
- **Decision**: Each handler in the chain evaluates the request. It can either:
	- Handle the request itself.
	- Pass the request to the next handler in the chain.
- **Chain termination**: The process stops when a handler successfully processes the request or when the request reaches the end of the chain without being handled. 

![[UML Class Diagram for Chain of Responsibility.png|550]]

**Common use cases**
- *Loggers*: Different loggers can be chained together, with each logger handling messages above a certain severity level and passing others down the chain.
- *Approvals*: An expense report can be passed from a junior manager to a senior manager based on the amount, forming a chain of approval.
- *Filters*: In web applications, requests can be passed through a chain of filters, with each filter performing a specific task like authentication or logging. 