#system-design-v2 #high-level-design

📌 Summary
> <mark style="background: #33EE99;">A resilience pattern that prevents cascading failures by stopping calls to a failing dependency and allowing it time to recover.</mark>

---
### 🎯 Problem Statement
- In distributed systems, services depend on other services over the network
- If a downstream service is slow or unavailable, callers keep retrying. This leads to:
	- Thread exhaustion
	- Increased latency
	- Cascading failures across the system

Example:
Let's assume we have 2 microservices - `Order Service` and `Product Service`. 
`Order Service` calls `Product Service`, but `Product Service` is down. 
Now any call made to `Product Service` is unnecessary but still `Order Service` keeps calling to `Product Service` which makes it harder for `Product Service` to recover quickly
![[Circuit Breaker - Problem in Microservice Example.png|850]]
### 🧠 Concept
- A circuit breaker prevents a service from making calls to another service, if it is down
- Based on failure thresholds, it either:
	- Allows calls
	- Blocks calls
	- Allows limited trial calls

![[Circuit Breaker Solution.png]]

#### States of Circuit Breaker
![[States of Circuit Breaker.png|850]]

#### Spring Boot
```xml
<!-- Resilience4j Circuit Breaker -->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-spring-boot3</artifactId>
</dependency>

<!-- Optional but recommended -->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-micrometer</artifactId>
</dependency>

<!-- Actuator for health & metrics -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```java
@Service
public class OrderService {

	@Autowired
	private ProductClient client;   //feign client that calls product service
	
	@CircuitBreaker(name = "productService", fallbackMethod = "fallback")  //name should match with application.properties name
	public void createOrder() {
		//some logic
		client.callProductAPI();    //call to product service
	}
	
	public void fallback(Throwable ex){
		log.error("Product Service is down");
	}
}
```

```yaml
resilience4j:
  circuitbreaker:
    instances:
      productService:   #should match the name in @CircuitBreaker
	    # register health in actuator  
        registerHealthIndicator: true
        
        # tracks last 10 calls to determine failure
        slidingWindowType: COUNT_BASED
        slidingWindowSize: 10
        
        # threshold which determines whether to move to open state
        failureRateThreshold: 50
        minimumNumberOfCalls: 5
        
        # move automatically to Half-Open after 10s
        automaticTransitionFromOpenToHalfOpenEnabled: true
        waitDurationInOpenState: 10s
        
        permittedNumberOfCallsInHalfOpenState: 3
        
```
##### Reference:
Shreyansh Jain Notes: https://onedrive.live.com/personal/6b364628c29feb52/_layouts/15/Doc.aspx?sourcedoc={92637273-69a9-4fcd-83cb-ce0c0db8053c}&action=view&redeem=aHR0cHM6Ly8xZHJ2Lm1zL28vYy82YjM2NDYyOGMyOWZlYjUyL0VuTnlZNUtwYWMxUGc4dk9EQTI0QlR3Qk5seWhEdlJlRWctS0RWWjdNZV9aWkE&wd=target%28New%20Section%201.one%7C1c705580-121a-3746-9837-b97f7646a06a%2FCircuit%20Breaker%20Fault-Tolerant%20Microservice%20%28Part-%7Caff21d09-7e7c-4a43-b09c-50e7a2c70e82%2F%29&wdorigin=NavigationUrl

But hey, even better - https://youtu.be/b6R4dElDtRc