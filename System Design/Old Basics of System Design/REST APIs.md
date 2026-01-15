#system-design #REST 

REST stands for **RE**presentational **S**tate **T**ransfer and is used to send information/data from backend to frontend/UI in a format that is acceptable at both ends

### Designing RESTful APIs - Guidelines

- **Use Nouns for Resources**: Represent resources with nouns, not actions (e.g., `/users`, not `/getUsers`).
- **HTTP Methods for Actions**: Use HTTP methods (GET, POST, PUT, DELETE) to define actions.
- **Statelessness**: Each request should contain all information necessary to process it, without relying on server-side sessions.
- **URI Hierarchy and Clarity**: Keep URLs clear and hierarchical, reflecting resource structure (e.g., `/users/123/orders`).
- **Representation**: Resources are sent in formats like **JSON** or **XML**.
- **Client-Server Architecture**: Separation of concerns - frontend and backend can evolve independently.    
- **Consistent Responses**: Use standard status codes (200 OK, 404 Not Found) and uniform response formats (JSON or XML).
- **Versioning**: Include versioning (e.g., `/api/v1/`) to manage API changes over time.
- **Documentation**: Provide clear API documentation for client developers.
- **Cacheable**: Responses can be cached to improve performance - prevents redundant processing by storing response data, reducing load on servers and improving client speed.
- **Layered System**: The architecture should support layered components (like proxies, load balancers) between clients and servers without impacting functionality. Each layer should operate independently, improving scalability and flexibility.

^REST-API-Guidelines

%% DO NOT REMOVE THE BELOW H3 - Keep it as it is even though it looks repetitive %% ^7bf3d1
### REST API in Spring Boot

![[Spring Boot Annotation#📌Spring MVC & REST]]

%% DO NOT REMOVE THE BELOW H3 - Keep it as it is even though it looks repetitive %%
### Swagger in Spring Boot
![[Swagger in Spring Boot]]