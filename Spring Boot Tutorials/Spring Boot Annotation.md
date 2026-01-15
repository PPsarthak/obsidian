---
status: Done
last-reviewed: 2025-08-23
tags:
  - spring-boot
  - annotations
---
 #spring-boot #annotations
### 📌Core
- `@SpringBootApplication` :`@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`
	- `@Configuration`
		- Indicates that the class can be used by the Spring IoC container as a source of bean definitions.
		- The class can contain `@Bean` methods to register beans.
	- `@EnableAutoConfiguration`
		- Spring Boot will register and configure beans automatically. It looks at the libraries/dependencies + `application.properties/application.yaml` file in on your project. If a particular library is present (e.g., Spring MVC, JPA, a database driver), Spring Boot will auto-configure beans appropriate for that library. Otherwise, in Spring, you would have to write a `@Configuration` file and explicitly add beans for `DataSource, ConnectionPool, EntityManagerFactory`
	- `@ComponentScan`
		- Enables component scanning so that Spring can discover beans annotated with stereotypes (e.g., `@Component, @Service, @Repository, @Controller`)
### 📌Spring Application Properties
1. `@ConfigurationProperties`: Binds external config to a Java object
2. `@Value`: Injects values from properties files
3. `@Profile`: Activates beans only for specific profiles.
	- `@Profile`: Applies at the bean or configuration class level. You decide exactly which beans load under which profiles.
	- `spring.profiles.active`: Applies globally. It only determines which of the pre-defined profiles are active for the whole application. It doesn’t target individual beans directly.
	- You can group multiple profiles (separated by comma), common values include - Dev, Test, QA, Stage, Prod
4. `@ConditionalOnProperty`: Loads a bean only if a specific property exists
### 📌Component Scanning & Dependency Injection
1. `@Component`:  Marks a Java class as a Spring-managed component.
	- Generic stereotype annotation in Spring which marks a class as a Spring-managed bean. Spring automatically detects it during component scanning and registers it in the application context.
	- Used for any class that doesn’t fit into a specific layer stereotype (`@Service, @Repository, @Controller`)
2. `@Service`: Specialized `@Component` for service layer (class contains business logic / service layer code)
3. `@Repository`: Specialized `@Component` for a class that is DAO/persistence layer 
	- Adds Spring’s automatic exception translation - Converts low-level persistence exceptions (like `SQLException`) into Spring’s `DataAccessException` hierarchy
4. `@Controller`: Specialized `@Component` for a Spring MVC controller that handles web requests and returns views (like JSP, Thymeleaf)
5. `@RestController`: Combines `@Controller` and `@ResponseBody`
	- Used in RESTful APIs where methods return JSON/XML directly instead of a view
	- Every method automatically serializes the return value into the response body which eliminates the need to put `@ResponseBody` on each method.
6. `@ComponentScan`: Enables component scanning in specified packages
	- If you place a class outside the base package, it won’t be scanned automatically. In such cases, you must explicitly configure. You can specify base packages explicitly - `@ComponentScan(basePackages = "com.ideas.fds")`
	- By default, `@ComponentScan` will scan the package of the class annotated with` @SpringBootApplication` and all its sub-packages recursively
```less
com.example.demo   ← main application class here (@SpringBootApplication)
 ├─ controller/
 ├─ service/
 ├─ repository/
 └─ entity/
```
7. `@Autowired`: Injects dependencies automatically
8. `@Qualifier`: Specifies which bean to inject when multiple candidates exist
	- Without it, Spring throws `NoUniqueBeanDefinitionException` if multiple candidates exist
```java
@Autowired
@Qualifier("specificBeanName")
private MyService service;
```
9. `@Primary`: Indicates the default bean to inject when multiple candidates are present
	- Used to avoid always specifying `@Qualifier`
	- If both `@Primary` and `@Qualifier` are present → **`@Qualifier`takes precedence**
```java
@Bean
@Primary
public PaymentService defaultPaymentService() {
    return new PaypalService();
}
```
### 📌Configuration & Bean Management
1. `@Configuration`: Indicates that the class contains Spring bean definitions (equivalent to the old XML-based configuration - beans.xml)
2. `@Bean`: Declares a method whose return object is registered as a Spring bean
	- Bean name **defaults to the method name**, but can be overridden
```java
@Bean("customBeanName")
public MyService myService() {
    return new MyService();
}
```
3. `@Lazy`: Creates the bean only when it's first requested (normally, Spring creates all singleton beans at startup)
	- Can be applied at:
		- **Class level** → All beans inside are lazy 
		- **Bean/method level** → Only that bean is lazy
4. `@DependsOn`: Specifies dependent beans that must be initialized first
```java
@DependsOn("dataSource")
@Bean
public MyRepository repository() { ... }
```
5. `@Scope`: Defines the lifecycle and visibility of a bean in the Spring container 
	- **singleton** (default): One shared instance for the entire context.
	- **prototype**: New instance created every time it’s requested.
	- Web scopes (in Spring Web applications):
		- **request**: One instance per HTTP request.
		- **session**: One instance per HTTP session.
		- **application**: One instance per Servlet Context.
```java
@Bean
@Scope("prototype")
public MyBean myBean() { ... }
```
### 📌Spring MVC & REST
1. `@RestController`: Combines `@Controller` and `@ResponseBody`, designating the class as a REST controller where each method returns JSON or XML directly.
2. `@RequestMapping:` Maps HTTP requests to handler methods.
3. `@GetMapping,@PostMapping, @PutMapping, @DeleteMapping`: Shorthand for `@RequestMapping`with HTTP methods.
4. `@RequestParam`: Binds HTTP query parameters to method arguments.
	- `url :- http://localhost:8080/api/foos?id=abc`
	- We can also configure our request param to be optional, though, with the `required=false` attribute 
5. `@PathVariable`: Binds URI template variables to method parameters. 
	- You can omit the name if the method parameter name matches the path variable
6. `@ResponseStatus`: Sets the HTTP status code for a response (e.g., 201 for resource creation)
7. `@RequestBody`: Binds HTTP request body to a method parameter.
8. `@ResponseBody`: Indicates the return value should be serialized to the HTTP response body.

> The `@ResponseBody` annotation enables automatic serialization of the return object into the `HttpResponse`
> The `@RequestBody` annotation  enables automatic deserialization of the incoming`HttpRequest` body to a domain object

9. `@RequestHeader`: Used to extract values from HTTP headers in an incoming request and bind them to method parameters in a controller
```Java
@GetMapping("/greet")
public String greetUser(@RequestHeader(name = "User-Agent") String userAgent) {...}

@GetMapping("/hello")
public String sayHello(@RequestHeader(name = "X-User", required = false, defaultValue = "Guest") String user) {...}

@GetMapping("/headers")
public String getAllHeaders(@RequestHeader Map<String, String> headers) {...}
```

10. `@CookieValue`: Used to bind the value of an HTTP cookie to a method parameter in a controller.
```Java
@GetMapping("/dashboard")
public String showDashboard(@CookieValue(name="session_id") String sessionId) {...}

@GetMapping("/welcome")
public String welcomeUser(@CookieValue(name="user", required=false, defaultValue="Guest") String user) {...}
```
### 📌JPA & Transaction
1. `@Entity`: Marks a class as a JPA entity
2. `@Table`: Maps the entity to a database table
3. `@Column`: Maps a field to a database column
4. `@Id`: Marks a field as a primary key
5. `@GeneratedValue`: Defines primary key generation strategy
6. `@OneToMany, @ManyToOne`: Defines relationships between entities
7. `@Transactional`: Declares a method or class as transactional
8. `@EnableTransactionManagement`: Enables annotation-driven transaction management
	- **In Spring (not Boot), you normally add `@EnableTransactionManagement` to a `@Configuration` class.** This tells Spring to look for @Transactional annotations and create AOP proxies around those methods. 
	- In Spring Boot, you don't have to add it yourself. Spring Boot auto-configures transaction management if it detects a transaction manager bean i.e., internally enables `@EnableTransactionManagement` for you.
	- What happens under the hood:
		- Spring sees `@Transactional`.
		- It creates a proxy around the bean.
		- When you call this method → proxy starts a transaction before execution.
		- If the method completes normally → commits the transaction. If it throws a runtime exception → rolls back.
9. `@EnableJpaRepositories`: Enables JPA repository scanning which automatically detects interfaces that extend `JpaRepository` or `CrudRepository`
```java
@EnableJpaRepositories(basePackages = "com.example.repo")
```
11. `@Query`: Defines a custom JPQL/SQL query
	- If `nativeQuery = true` is added → query is written in raw SQL.
12. `@Modifying`: Used along with @Query for modifying operations (update/delete). Without it, Spring expects queries to be select only. Must be combined with @Transactional for changes to persist.
```java
@Modifying
@Transactional
@Query("UPDATE User u SET u.active = false WHERE u.lastLogin < ?1")
int deactivateOldUsers(LocalDate cutoff);
```
### 📌 Spring Testing
1. `@SpringBootTest`: Loads a full application context for integration testing (across multiple layers)
2. `@DataJpaTest`: Configures an in-memory database and tests only JPA components (excludes web/service layer by default)
3. `@WebMvcTest`: Loads only web layer (controllers) for testing; does not load services, repositories, etc. (must be mocked)
4. `@MockBean`: Adds Mockito mock into the Spring `ApplicationContext` (which replaces the actual bean)
5. `@TestConfiguration`: Defines extra beans specifically for tests
```java
@TestConfiguration
static class TestConfig {
    @Bean
    PasswordEncoder passwordEncoder() {
        return NoOpPasswordEncoder.getInstance();
    }
}
```

6. `@AutoConfigureMockMvc`: Enables and injects MockMvc for testing web endpoints (not ever used by me)
7. `@Sql`: Runs SQL scripts before or after tests (good for preparing test data)
```java
@Sql(scripts = "classpath:test-data.sql")
class MyRepoTest { }
```

8. `@ParameterizedTest` (JUnit 5) : Runs the same test with different parameters. Useful for multiple inputs/outputs
```java
@ParameterizedTest
@ValueSource(strings = {"admin", "user", "guest"})
void testRoles(String role) { ... }
```
### 📌Scheduling & Async Execution
1. `@EnableScheduling`: Enables Spring’s scheduled task execution capability
	- Must be added to a configuration class (commonly your main @SpringBootApplication class)
	- Allows methods annotated with @Scheduled to run automatically
2. `@Scheduled`: Declares a method to be scheduled task
	- `fixedRate` → runs repeatedly at a fixed interval (measured from start time)
	- `fixedDelay` → runs repeatedly after a fixed delay from the previous completion
	- `cron` → uses a cron expression for complex schedules
3. `@EnableAsync`: Enables asynchronous method execution. Must be added to a configuration class
4. `@Async`: Marks a method to run asynchronously in a separate thread
	- Returns either:
		- `void` → fire-and-forget
		- `Future<T>`/`CompletableFuture<T>` → async result that can be awaited later
		
### 📌Spring AOP (Aspect-Oriented Programming)
1. `@EnableAspectJAutoProxy`: Enables support for handling components marked with `@Aspect`
2. `@Aspect`: Marks a class as an aspect. Aspects contain advices (@Before, @After, etc.) and pointcuts
3. `@Before`: Executes before a matched method
4. `@After`: Executes after a matched method
5. `@AfterReturning`: Executes after a method returns successfully. Can capture the return value with returning attribute
6. `@AfterThrowing`: Executes if a matched method throws an exception. Can capture the exception with throwing attribute
7. `@Around`: Wraps method execution (before and after). Has control to proceed or skip method execution.
8. `@Pointcut`: A pointcut defines where advice applies. 
	- Can also be declared inline (inside an advice annotation). E.g.,
	```java
	@Pointcut("execution(* com.example.service.*.*(..))")
	public void serviceMethods() { }
	
	@Before("serviceMethods()")
	public void logBefore() { ... }
	```
	- Common pointcut expressions:
		- `execution(* com.example.service.*.*(..))` → any method in service package.
		- `within(com.example.service..*)` → all methods in classes under package.
		- `args(java.lang.String,..)` → methods where first argument is String.
		- `@annotation(org.springframework.transaction.annotation.Transactional) `→ methods annotated with @Transactional.
### 📌Exception Handling
1. `@ControllerAdvice`: Handles exceptions across multiple controllers and <mark style="background: #33EE99;">works like an interceptor for exceptions thrown by `@Controller` or `@RestController`</mark>
```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleAll(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
			 .body("Something went wrong!");
    }
}
```
2.  `@ExceptionHandler`: Defines method to handle specific exceptions. Can be applied inside a `@Controller` or in a `@ControllerAdvice` class
	- Supports inheritance → a handler for `RuntimeException` will also catch subclasses.
3. `@ResponseStatus`: Sets the HTTP status for a method or exception.
```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String msg) { super(msg); }
}

@GetMapping("/ping")
@ResponseStatus(HttpStatus.NO_CONTENT)
public void ping() { }
```
### 📌Spring Cloud & Microservices
1. `@EnableDiscoveryClient`: Enables service registration & discovery (Eureka, Consul, Zookeeper). Registers the service with the discovery server and allows it to look up other services. Typically placed on the main application class.
2. `@FeignClient`: Declarative REST client for inter-service communication (auto-generates an implementation for calling another service). Works with service discovery (uses service name instead of hardcoded URL)
3. `@LoadBalanced`: Adds client-side load balancing (to RestTemplate bean). Allows calling other services via service name (instead of host : port)
4. `@RefreshScope`: Allows beans to be reloaded at runtime when configuration changes (from Config Server). Useful for dynamic config updates without restarting the service.
5. `@EnableConfigServer`: Starts a Spring Cloud Config server.
### 📌Spring Kafka
1. `@EnableKafka`: Enables Kafka-related annotations. Usually placed on the main application class or a configuration class
2. `@KafkaListener`: Marks a method as a Kafka message listener and subscribes to the given topic(s)
3. `@KafkaHandler`: Allows multiple handler methods inside a single listener class (like method overloading for messages). Dispatches messages to the method based on payload type
	- Used with class-level `@KafkaListener` for multiple methods.
```java
@KafkaListener(topics = "events")
public class EventListener {
    
    @KafkaHandler
    public void handleOrder(Order order) { ... }

    @KafkaHandler
    public void handlePayment(Payment payment) { ... }

    @KafkaHandler(isDefault = true)
    public void handleDefault(Object obj) { ... }
}
```
### 📌Messaging & Web Sockets
`@EnableWebSocket`: Enables WebSocket configuration
`@MessageMapping`: Maps incoming messages to methods (used in STOMP)
`@SendTo`: Sends response message to a specific topic
`@SubscribeMapping`: Handles subscription requests
`@JmsListener`: Listens to JMS messages
`@RabbitListener`: Listens to RabbitMQ messages
### 📌Caching
`@EnableCaching`: Enables Spring's annotation-driven cache management
`@Cacheable`: Caches the result of a method
`@CachePut`: Updates the cache with the method result
`@CacheEvict`: Removes an entry from the cache
### 📌Spring Security
`@EnableWebSecurity`: Enables Spring Security configuration.
`@PreAuthorize`: Method-level security using expressions.
`@Secured`: Defines security constraints on methods.
`@WithMockUser`: Mocks a user for testing purposes.
### 📌 Spring Data MongoDB
`@Document`: Marks a class as a MongoDB document
`@Field`: Maps a field to a specific MongoDB field name
`@Id`: Marks the primary key of the document 
`@DBRef`: References another document 
### 📌Spring Data Cassandra
`@Table`: Marks a class as a Cassandra table
`@PrimaryKey`: Marks the primary key field
`@Column`: Maps a field to a Cassandra column
`@CassandraType`: Defines the Cassandra-specific data type
### 📌 Validation
1. `@Valid`: Triggers validation on method arguments
```Java
public ResponseEntity<?> register(@Valid @RequestBody User user) { ... }
//So when we add @Valid for method argument, that object will be validated for the below constraints
```
2.  `@NotNull`: Validates that a value is not null. <mark style="background: #19B5FF;">Does not check for empty</mark> strings or empty collections (use `@NotBlank` / `@NotEmpty` for that).
3. `@Size`: Validates size constraints (length or collection size). Requires both min and/or max
4. `@Email`: Validates that a field contains a valid email address
5. `@Pattern`: Validates a string against a regex
6. `@Min, @Max`: Validates numeric boundaries
7. `@NotBlank`: Validates non-null, non-empty, and non-whitespace string.
8. `@NotEmpty`: Validates non-null and non-empty (works for strings, collections, arrays)
9. `@Positive, @PositiveOrZero, @Negative, @NegativeOrZero` → Validates numeric sign.
10. `@Future, @Past, @FutureOrPresent, @PastOrPresent` → Validates date/time fields.

> ⚡ Typically, these are combined with `@Valid` in a DTO and validated automatically when handling requests

```java
@NotNull
private String username;

@Size(min = 8, max = 20)
private String password;

@Email
private String email;

@Pattern(regexp="^[0-9]{10}$")
private String phone;

@Min(18)
@Max(65)
private int age;

@NotBlank
private String name;   // must not be null, empty, or just spaces

@NotEmpty
private List<String> roles;   // must not be null and must have at least one element

@Positive
private int quantity;   // must be > 0

@PositiveOrZero
private int stock;      // must be >= 0

@Negative
private int temperature;   // must be < 0

@NegativeOrZero
private int balance;       // must be <= 0

@Future
private LocalDate expiryDate;   // must be in the future

@Past
private LocalDate birthDate;    // must be in the past

@FutureOrPresent
private LocalDate appointment;  // today or a future date

@PastOrPresent
private LocalDate joinedOn;     // today or a past date
```