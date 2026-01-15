---
status: Done
last-reviewed: 2025-09-07
tags:
  - spring-boot
  - config-server
---
#spring-boot #config-server
### Outline

- Dependency & Setup  - [[#Dependency & Setup]]
- What is Spring Cloud Config Server? - [[#What is Spring Cloud Config Server?]]
- How Config Server Works? - [[#How Config Server Works]]
- Centralized Properties Management - [[#Centralized Properties Management]]
- Environment-Specific Configuration - [[#Environment-Specific Configuration]]
- Dynamic Refresh of Configuration - [[#Dynamic Refresh of Configuration]]
- Consistency Across Microservices - [[#Consistency Across Microservices]]
- Version Control and Auditability - [[#Version Control and Auditability]]
- Integration with Eureka & API Gateway - [[#Integration with Eureka & API Gateway]]
### Dependency & Setup
1. Add Dependency
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

2. Enable Config Server
```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

3. Configure properties
```properties
spring.application.name=config-server
server.port=8888

# Git repository for storing configuration files
spring.cloud.config.server.git.uri=https://github.com/your-org/config-repo
spring.cloud.config.server.git.clone-on-start=true
spring.cloud.config.server.git.default-label=main
spring.cloud.config.server.git.search-paths=services
```
- uri → URL of the Git repository containing configuration files.
- clone-on-start → Ensures repository is cloned when server starts.
- default-label → Branch to read from (e.g., main or master).
- search-paths → Optional folder paths within the repo.

4. Structure of configuration files in Git
```bash
config-repo/
├── application.yml           # default configuration for all services
├── payment-service.yml       # config specific to payment-service
├── order-service.yml         # config specific to order-service
├── application-dev.yml       # dev environment overrides
└── application-prod.yml      # prod environment overrides
```

5. Accessing Config Server
- Run the Config Server → access via URL (`http://localhost:8888/{application}/{profile}`) - http://localhost:8888/payment-service/dev

### What is Spring Cloud Config Server?
#### Centralized Configuration Management
- Manages all configuration properties for microservices from a central location.
- Eliminates the need to hardcode properties in each service.
- Supports multiple environments (dev, test, prod) from the same repo.
#### Role in Microservices Architecture
- Microservices register themselves with Eureka or run independently.
- Instead of storing configurations locally, each service fetches configs from Config Server at startup.
- Provides a single source of truth for properties across all services.
#### Key Benefits
- Consistency: All services use the same configuration version.
- Flexibility: Update configuration centrally without redeploying each service.
- Environment-specific management: Separate dev/test/prod configs in the same repo.
- Dynamic refresh: Works with Spring Cloud Bus to push updates at runtime

### How Config Server Works
#### Service Startup
- Each client microservice includes spring-cloud-starter-config dependency.
- Client points to Config Server (spring.cloud.config.uri=http://localhost:8888).
- Client fetches its configuration using its spring.application.name and active profile.

#### Property Resolution
- Config Server serves default properties from properties file
- Overrides with application-specific or profile-specific configs.
- Resolves properties in the following order (highest priority first):
	1. `application-{profile}.yml`
	2. {`service}-{profile}.yml`
	3. `application.yml`
- Client receives final merged configuration at startup.
#### Dynamic Refresh
With Spring Actuator and Spring Cloud Bus:
- Config Server can push updates to clients.
- Clients automatically refresh properties at runtime without restarting.

### Use Cases
#### Centralized Properties Management
Multiple microservices with separate `application.yml` files → inconsistent values, hard to manage.
Solution:
- Store all properties centrally in Git or filesystem.
- Clients fetch config from Config Server.
- Single source of truth, easier auditing, versioning, and rollback.
#### Environment-Specific Configuration
Services need different configurations for dev, test, prod.
Solution:
- Use `{application}-{profile}.yml` files to define environment-specific values.
- Activate profile via `spring.profiles.active` in the client.
- Example:
	- `payment-service-dev.yml `→ database URL: `jdbc:mysql://dev-db:3306/payments`
	- `payment-service-prod.yml` → database URL: `jdbc:mysql://prod-db:3306/payments`
#### Dynamic Refresh of Configuration
Updating configs requires redeploying services → downtime risk.
Solution:
- Spring Cloud Config + Actuator + Bus → push config changes dynamically.
- Use /actuator/refresh endpoint or bus events to update clients without restart.
- Minimal downtime, reactive configuration updates.
#### Consistency Across Microservices
Multiple microservices may rely on the same external resources (databases, message brokers).
Solution:
- Define shared properties` (application.yml)` for all clients.
- Overrides for service-specific values are still possible.
- Avoids drift in configurations, ensures alignment between services.
#### Version Control and Auditability
- Store configurations in Git → track every change.
- Rollback is easy by switching to a previous commit or branch.
- Enables audit trails for compliance in regulated environments.
#### Integration with Eureka & API Gateway
- Eureka: Services register themselves → fetch dynamic configs via Config Server.
- API Gateway: Routes requests dynamically → can use Config Server to adjust routing, timeouts, or filters without redeploying.

### **Quick Recap / Key Takeaways**

Dependency & Setup
- Add spring-cloud-config-server dependency and annotate with @EnableConfigServer.
- Configure backend (Git or native filesystem).
- Structure config files by service and profile.

Concept
- Centralized, version-controlled configuration management for microservices.
- Eliminates hardcoding and ensures consistency across environments.
- Supports dynamic refresh with Spring Cloud Bus and Actuator.

Use Cases
- Centralized management of microservice properties.
- Environment-specific configs (dev, test, prod).
- Dynamic updates without redeployment.
- Consistent configuration across multiple services.
- Version control and rollback via Git.
- Works with Eureka and API Gateway for fully dynamic microservice ecosystems.

Why it matters
- Simplifies microservices maintenance.
- Improves flexibility, scalability, and reliability.
- Keeps business logic separate from environment-specific configuration.