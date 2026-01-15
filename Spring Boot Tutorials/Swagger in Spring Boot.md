---
status: Done
last-reviewed: 2025-08-30
tags:
  - spring-boot
  - swagger
---
#spring-boot #swagger #placement-preparation 
### What is Swagger?
- Swagger (OpenAPI) is a framework for describing, documenting, and testing REST APIs.
- It automatically generates interactive API documentation where developers can see endpoints, try them out, and understand request/response formats
- It is actually more than just docs:
	- Provides live testing console.
	- Helps with API design & contract-first development.
	- Generates client SDKs and server stubs in multiple languages.
##### Dependency 
```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.5.0</version>
</dependency>
```
Just add this dependency and start the application. This gives you:
- API docs at: `/v3/api-docs` (JSON format).
- Swagger UI at: `/swagger-ui.html` (interactive web UI)
#### 🏷️ Common Annotations
- API Documentation
	- *`@OpenAPIDefinition`* → Defines general info about your API (title, version, description).
	- *`@Info`* → Metadata like API title, description, version.
	- *`@Tag`* → Groups endpoints (like "Users API", "Orders API")
- Endpoint Documentation
	- *`@Operation`* → Describes an endpoint - summary, description, tags, etc.
		- Example - `@Operation(summary = "Get user by ID", description = "Returns a single user")`
	- *`@ApiResponse`* → Documents HTTP response codes & descriptions.
		- Example - `@ApiResponse(responseCode = "200", description = "User found")`
- Parameter & Request Body
	- *`@Parameter`* → Describes query/path/header params
		- Example - `@Parameter(description = "ID of user to fetch", required = true)`
	- *`@RequestBody`* → Describes request body payload
- Model Documentation
	- *`@Schema`* → Used inside DTO/entity classes to describe fields
		- Example :
```java
@Schema(description = "The unique ID of the user", example = "42")
private Long id;
```