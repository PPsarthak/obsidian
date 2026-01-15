---
status: Pending
last-reviewed: 2025-08-30
tags:
  - spring
  - microservices
---

#placement-preparation #spring-boot #microservices #active 

Topics Covered:
- Synchronous vs Asynchronous
- REST (synchronous, simple)
- Feign Client (declarative REST client)
- Kafka (asynchronous messaging)
- gRPC (fast binary communication)
#### Synchronous vs Asynchronous
<mark style="background: #FF5582A6;">Synchronous</mark>
- Real-time, blocking communication (The caller waits for the response)
- Commonly implemented using HTTP (REST APIs) or gRPC
- ✅ Simple to implement  
- ❌ Tightly coupled, slower under load, potential cascading failures

<mark style="background: #FF5582A6;">Asynchronous</mark>
- Non-blocking communication.
- The caller sends a message and moves on without waiting for a response.
- Implemented using message brokers like Kafka, RabbitMQ, or AWS SNS/SQS.
- ✅ Loosely coupled, scalable, better fault tolerance  
- ❌ More complex to debug and design (eventual consistency, retries, etc.
#### RestTemplate
 <mark style="background: #19B5FF; color: #FFFFFF" >RestTemplate is considered legacy</mark> and Spring recommends using `WebClient` (reactive, non-blocking) for new applications.
 However, `RestTemplate` is still widely used in synchronous microservices.

Example - 
```Java
@RestController
public class OrderController {
	@Autowired
    private final RestTemplate restTemplate;

    @GetMapping("/order/{id}")
    public Product getProductById(@PathVariable String id) {
        String url = "http://product-service/products/" + id;
        return restTemplate.getForObject(url, Product.class);
    }
}
```
##### Common `RestTemplate` Methods

| Method            | Description                         |
| ----------------- | ----------------------------------- |
| `getForObject()`  | GET request, returns body only      |
| `getForEntity()`  | GET request, returns full response  |
| `postForObject()` | POST request, returns response body |
| `postForEntity()` | POST request, full response         |
| `put()`           | PUT request, no return              |
| `delete()`        | DELETE request                      |
| `exchange()`      | Generic method for all HTTP verbs   |

```Java
HttpHeaders headers = new HttpHeaders();
headers.setContentType(MediaType.APPLICATION_JSON);
headers.set("Authorization", "Bearer " + jwtToken);

HttpEntity<ProductRequest> requestEntity = new HttpEntity<>(new ProductRequest("Shoes", 1999), headers);

ResponseEntity<ProductResponse> response = restTemplate.exchange(
    "http://product-service/products",
    HttpMethod.POST,
    requestEntity,
    ProductResponse.class
);
```
---
#### `WebClient`
- WebClient is part of the Spring WebFlux module. 
- It supports reactive programming and is recommended for non-blocking, asynchronous communication between microservices.

###### Maven Dependency:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

###### Creating a WebClient Instance
```java
@Configuration
public class WebClientConfig {

    @Bean
    public WebClient webClient(WebClient.Builder builder) {
        return builder
                .baseUrl("http://localhost:8081")  // Default base URL
                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .build();
    }
}

//Instead of WebClient.Builder you can also use:

WebClient client = WebClient.create();
//OR
WebClient client = WebClient.create("http://localhost:8080");
//OR
WebClient client = WebClient.builder().baseUrl("http://localhost:8080")
  .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE).build();
```
###### Calling the URL
```java
public Mono<Product> getProduct(String productId) {
	return webClient.get().uri("/products/{id}", productId)
			.retrieve().bodyToMono(Product.class);
}

public Flux<Product> getAllProducts() {
	return webClient.get().uri("/products")
			.retrieve().bodyToFlux(Product.class);
}

public Mono<Product> createProduct(Product product) {
	return webClient.post().uri("/products")
			.bodyValue(product).retrieve().bodyToMono(Product.class);
}
```

**Key APIs in WebClient**
`.get() / .post() / .put() / .delete()` → HTTP verbs
`.uri()` → Build URIs with placeholders
`.bodyValue(object)` → Send request body
`.retrieve() `→ Trigger the request and handle response
`.bodyToMono(Class)` → Expect a single object
`.bodyToFlux(Class)` → Expect a stream/list of objects
`.exchangeToMono()` → Advanced handling (status codes, headers, etc.)
###### Handling Errors
```java
public Mono<Product> getProductSafe(String productId) {
    return webClient.get().uri("/products/{id}", productId).retrieve()
		.onStatus(HttpStatusCode::is4xxClientError,
			response -> Mono.error(new RuntimeException("Product not found")))
		.onStatus(HttpStatusCode::is5xxServerError,
			response -> Mono.error(new RuntimeException("Server error")))
		.bodyToMono(Product.class);
}
```

###### Synchronous Usage
```java
Product product = webClient.get().uri("/products/{id}", 123)
        .retrieve().bodyToMono(Product.class)
        .block();
```

##### Best Practices in Microservices
1. Use Service Discovery (Eureka, Consul)
	- Don’t hardcode base URLs.
	- Use load balancing with `spring-cloud-starter-loadbalancer`
```java
@Bean
@LoadBalanced
public WebClient.Builder loadBalancedWebClientBuilder() {
    return WebClient.builder();
}
//Then use service name:
WebClient webClient = webClientBuilder.baseUrl("http://PRODUCT-SERVICE").build();
```

2. Resilience4J [[System Design/HLD/Circuit Breaker|Circuit Breaker]]
```java
@CircuitBreaker(name = "productService", fallbackMethod = "fallbackProduct")
public Mono<Product> getProduct(String productId) {
    return webClient.get()
            .uri("/products/{id}", productId)
            .retrieve()
            .bodyToMono(Product.class);
}
```
3. Centralized Config
	- Externalize base URLs and timeouts using Spring Cloud Config Server.
---
#### Feign Client
- **Feign** is a declarative web service client developed by Netflix (now maintained under Spring Cloud).  
- It simplifies HTTP calls between microservices by allowing developers to write **Java interfaces** annotated with REST mappings instead of manually using `RestTemplate` or `WebClient`.  
- In other words → "You define the interface, Feign generates the implementation."

###### Add Dependency
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

###### Enable Feign Client
```java
@SpringBootApplication
@EnableFeignClients
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}
```

###### Define a Feign Interface
```java
@FeignClient(
    name = "payment-service", // registered service name in Eureka
    url = "http://localhost:8081" // optional if using discovery
)
public interface PaymentClient {

    @GetMapping("/payments/{id}")
    PaymentResponse getPayment(@PathVariable("id") Long id);

    @PostMapping("/payments")
    PaymentResponse createPayment(@RequestBody PaymentRequest request);
}
```

#### Kafka









#### gRPC






#### Websockets










---
