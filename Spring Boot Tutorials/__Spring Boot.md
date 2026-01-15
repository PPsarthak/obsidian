---
status: In Progress
last-reviewed: 2025-08-24
tags:
  - spring-boot
  - placement-preparation
---

![[Spring Boot Base.base]]

### 🔰 Introduction to Spring Boot
- What is Spring Boot?
- Why use Spring Boot?
- Difference between Spring and Spring Boot
- Features of Spring Boot
- Spring Boot Architecture Overview
---
### 2. 🧱 Project Setup & Structure
- Creating a Spring Boot project (via Spring Initializer / IDE)
- Understanding `pom.xml` / `build.gradle`
- Project directory structure
- `@SpringBootApplication` explained
- Application startup flow
---
### 3. ⚙️ Configuration Management
- `application.properties` vs `application.yml`
- Profiles: `@Profile`, `application-{profile}.yml`
- External configuration (env vars, command-line args)
- Using `@Value`, `@ConfigurationProperties`
- Default values and fallback mechanisms
---
### 4. 📦 Dependency Injection (DI)
- `@Component`, `@Service`, `@Repository`, `@Controller`
- `@Autowired`, constructor injection, field injection
- `@Qualifier`, `@Primary`
- Lifecycle of Beans (`@PostConstruct`, `@PreDestroy`)
- `@ComponentScan` and base packages
---
### 5. 🌐 Creating REST APIs (Basics)
- `@RestController`, `@RequestMapping`, `@GetMapping`, etc.
- Request and Response objects
- Path variables and query params
- `@RequestBody`, `@ResponseBody`
- HTTP status codes and `ResponseEntity`
---
### 6. 🛠 Spring Boot Dev Tools & Testing
- Spring Boot DevTools (auto-reload, LiveReload)
- Unit testing with `@SpringBootTest`, JUnit, and Mockito
- Integration tests
- Using `test` scope in `pom.xml`
---
### 7. 📄 Logging & Application Monitoring
- Default logging setup (Logback)
- Customizing log levels (`application.yml`)
- Logging to files
- `@Slf4j` and other logging annotations
---
### 8. 🧪 Validation & Exception Handling
- Bean validation using `@Valid`, `@NotNull`, etc.
- `@ExceptionHandler`, `@ControllerAdvice`
- Custom exception classes
---
### 9. 🧰 Spring Boot Actuator (Intro)
- What is Actuator?
- Enabling and customizing endpoints
- Health check, metrics, and info endpoints
---
### 10. 🐘 Working with Databases (Intro)
- Spring Data JPA overview
- `application.yml` DB config
- Using `@Entity`, `@Repository`
- H2 in-memory DB for testing
- `schema.sql`, `data.sql` for bootstrapping
---

Spring Boot Fundamentals for Microservices:
- Spring Boot Dev Tools, Actuator
- Spring Web, Spring Data JPA, H2/PostgreSQL
- Lombok, MapStruct, Validation (JSR-303)

Microservices Architecture Principles and Patterns:
- Service Decomposition
- Domain-Driven Design (DDD)
- Database per service
- API Gateway Pattern, Circuit Breaker Pattern, CQRS, Event Sourcing

Spring Cloud Ecosystem:
- Spring Cloud Config Server (externalized config)
- Eureka (Service registry)
- Spring Cloud Gateway or Zuul (API Gateway)
- Resilience4j (Circuit Breaker, Retry, RateLimiter)
- Sleuth + Zipkin (Tracing)

Inter service communication:
- REST (synchronous, simple)
- Feign Client (declarative REST client)
- Kafka (asynchronous messaging)
- gRPC (fast binary communication)

Microservice security:
- Spring Security
- OAuth2 + JWT (Resource Server, Auth Server)
- API Gateway Security (token relay, scopes)

Observability & Monitoring:
- Spring Boot Actuator
- Micrometer
- Zipkin / Sleuth (Tracing)
- Prometheus + Grafana (Metrics & Dashboards)
- ELK stack or Loki (Logging)


**What is Spring Boot?**
Spring Boot is built on top of Spring framework and simplifies the development of Spring applications

**What is a framework?**
A framework consists of libraries and is used to provide abstraction for developing applications. Think of it as a template, allowing developers to focus on the application's logic rather than low-level details

#### Inversion of Control and Dependency Injection 
In spring, the control of object creation and lifecycle is given to a container called as the IOC Container.

DI provide objects their dependencies rather than letting them instantiate the dependencies themselves. There are 3 ways to do so - 
- Constructor Injection
- Setter Injection
- Field Injection (via `@Autowired`)

##### Connection to MySQL DB
> Grab the dependency from start.spring.io

And add these in your `application.properties` file

```
spring.datasource.url=jdbc:mysql://localhost:3306/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
```

| @SpringBootApplication |     |
| ---------------------- | --- |
|                        |     |

#### IOC Container
In a nutshell, the IoC is a container that injects dependencies while creating the bean. IoC stands for ‘Inversion Of Control’.
Instead of creating objects by the developer, the bean itself controls the instantiation or association of its dependencies by using direct construction of classes with the help of the IoC container. Hence, this process is known as ‘Inversion of control’.

#### Application Context
When you create a project in Spring or Spring Boot, a container or wrapper gets created to manage your beans. This is nothing but Application Context.
However Spring supports two containers : Bean Factory and Application Context. 
In short, the `BeanFactory` provides the configuration framework and basic functionality. 
On the other hand, `ApplicationContext` adds more enterprise-specific functionality including easier integration with Spring’s AOP features; message resource handling (for use in internationalization), event publication; and application-layer specific contexts such as the `WebApplicationContext` for use in web applications.

`ApplicationContext` represents the Spring IoC container and it manages the process of instantiating, configuring, and assembling the beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. The configuration metadata can be represented in three formats: XML, Java annotations, or Java code.

#### Component Scanning and @ComponentScan
The process of discovering classes that can contribute to the Application Context, is called Component Scanning.
During Component Scanning, if Spring finds a class annotated with a particular annotation, It will consider this class as a candidate for Spring Bean/Component and adds it to the Application Context.

Spring explicitly provides a way to identify Spring bean candidates via the [@ComponentScan](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html) annotation.

By default, the @ComponentScan annotation will scan for components in the current package and its all sub-packages.
If it is a Spring Boot application, then all the packages under the package containing the Main class (a class annotated with [@SpringBootApplication](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/SpringBootApplication.html)) will be covered by an implicit component scan.
> So if your package doesn’t come under the hierarchy of the package containing the Main class, then there is a need for explicit component scanning.

### Core Spring
[@SpringBootApplication](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/SpringBootApplication.html):
- combination of 3 annotations with their default values - @Configuration, @ComponentScan, and @EnableAutoConfiguration.
- [@EnableAutoConfiguration](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/EnableAutoConfiguration.html): enables the auto-configuration feature of Spring Boot.
- [@ComponentScan](https://docs.spring.io/spring-framework/docs/6.2.x/javadoc-api/org/springframework/context/annotation/ComponentScan.html): enables @Component scan on the package to discover and register components as beans in Spring’s application Context.
- [@Configuration](https://docs.spring.io/spring-framework/docs/6.2.x/javadoc-api/org/springframework/context/annotation/Configuration.html): allows to register extra beans in the context or imports additional configuration classes.

[@Bean](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html) and [@Configuration](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html) -
- @Bean is for methods that returns the object managed by IOC Container
- @Configuration marks annotated class as a Java configuration defining beans
```Java
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyService();
    }
}

@SpringBootApplication
public class SpringBootComponentExample{

	//fetch the bean like this ...
	@Autowired
	private MyService myService;
	
    public static void main(String[] args) {
        var context = SpringApplication.run(SpringBootComponentExample.class, args);
	    
	    //or fetch the bean like this
        MyService mySer = (MyService) context.getBean(MyService.class);
    }
}
```
- so, the class contains @Configuration annotation and methods inside the class has @Bean annotation; these methods return the object 
- useful when you need customized beans

[@Component](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html):
- Marks annotated class as a bean found by the component-scanning and loaded into the application context
```Java
@Component
public class MyComponent {
    public String sayHello() {
        return "Hello from MyComponent!";
    }
}
```

> [!tip]
> You can also add names next to @Bean or @Component.
> This name can then be used to retrieve the bean from context.getBean() method
> For e.g., if you have @Component("casperService"), then you can get bean like this - context.getBean("casperService")

> [!tldr] @Bean vs @Component
@Component is a class level annotation whereas @Bean is a method level annotation and name of the method serves as the bean name. @Bean annotation has to be used within the class and that class should be annotated with @Configuration. However, @Component needs not to be used with the @Configuration. @Component auto detects and configures the beans using classpath scanning, 

[@Autowired](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html):
- used to inject the bean automatically through constructor injection, setter injection or field injection
```Java
public class MyComponent {

    private MyService myService; // Used for constructor & setter injection

    // Field Injection
    @Autowired 
    private MyService fieldInjectedService;

    // Constructor Injection (Recommended)
    @Autowired
    public MyComponent(MyService myService) {
        this.myService = myService;
    }

    // Setter Injection
    @Autowired
    public void setMyService(MyService myService) {
        this.myService = myService;
    }
}
```

[@Qualifier](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html):
- Used to resolve ambiguity when multiple beans of the same type exist.
- Works with `@Autowired` to specify which exact bean should be injected.
```Java
@Component("englishGreeting")
public class EnglishGreeting implements GreetingService {
    public String greet() {
        return "Hello!";
    }
}

@Component("spanishGreeting")
public class SpanishGreeting implements GreetingService {
    public String greet() {
        return "¡Hola!";
    }
}

@Component
public class GreetingComponent {
    private final GreetingService greetingService;

    @Autowired
    public GreetingComponent(@Qualifier("spanishGreeting") GreetingService greetingService) {
        this.greetingService = greetingService;
    }
}
```

[@Primary](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Primary.html):
- Used to set a default bean when multiple beans of the same type exist.
- If no `@Qualifier` is specified, Spring will inject the bean marked as `@Primary`.

```Java
@Component
@Primary
public class EnglishGreeting implements GreetingService {
    public String greet() {
        return "Hello!";
    }
}

@Component
public class SpanishGreeting implements GreetingService {
    public String greet() {
        return "¡Hola!";
    }
}

@Component
public class GreetingComponent {
    private final GreetingService greetingService;

    @Autowired
    public GreetingComponent(GreetingService greetingService) {
        this.greetingService = greetingService;       //English is used as it is marked Primary
    }
}
```

[@Value](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Value.html):
- Used to inject values from property files, environment variables, or default values into Spring beans.
```Java
@Component
public class AppConfig {

    @Value("${app.name}")
    private String appName;

    @Value("${app.version:1.0}") // Default value 1.0 if not found
    private String appVersion;

    @Value("${app.languages}")
    private List<String> supportedLanguages;  // Injects a comma-separated list

    public void displayConfig() {
        System.out.println("App Name: " + appName);
        System.out.println("Version: " + appVersion);
        System.out.println("Supported Languages: " + supportedLanguages);
    }
}
```








### Spring Web
- **@RestController**: Combines `@Controller` and `@ResponseBody`, designating the class as a REST controller where each method returns JSON or XML directly.
- **@RequestMapping**: Maps HTTP requests to handler methods based on URL paths and HTTP methods (e.g., GET, POST).
- **@GetMapping, @PostMapping, @PutMapping, @DeleteMapping**: Specialized mappings for HTTP methods, simplifying `@RequestMapping` usage.
- **@PathVariable**: Extracts variables from the URI, allowing dynamic endpoints (e.g., `/users/{id}`).
- **@RequestParam**: Maps query parameters to method arguments, useful for optional URL parameters.
- **@RequestBody**: Binds the request payload to a method parameter, often used for POST and PUT requests.
- **@ResponseStatus**: Sets the HTTP status code for a response (e.g., 201 for resource creation).

### Spring Test
- [@SpringBootTest](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/context/SpringBootTest.html) - Annotated test class will load the entire application context for integration tests
- [@WebMvcTest](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/web/servlet/WebMvcTest.html) - Annotated test class will load only the web layer (service and data layer are ignored)
- [@DataJpaTest](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/orm/jpa/DataJpaTest.html) - Annotated class will load only the JPA components
- [@JsonTest](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.testing.spring-boot-applications.json-tests) - Annotated class will load only json mapper for serialization and deserialization tests
- [@MockBean](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/mock/mockito/MockBean.html) - Marks annotated field as a mock and loads it as a bean into the application context
- [@SpyBean](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/mock/mockito/SpyBean.html) - Allows partial mocking of beans
- [@Mock](https://www.javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mock.html) - Defines annotated field as a mock