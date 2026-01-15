---
status: To Review
last-reviewed: 2025-09-18
tags:
  - spring-boot
  - web-flux
---
#spring-boot #web-flux 

> Reference: (https://howtodoinjava.com/spring-webflux/spring-webflux-tutorial/)
## Reactive Programming
#### The Problem
- Asynchronous, non-blocking I/O is important in scalable microservices where multiple requests needs to be served with minimum resources
- Handling live stream of data whose volume is not predetermined requires special care in asynchronous system
---

- Reactive programming is a programming paradigm that promotes an asynchronous, non-blocking, event-driven approach to data processing. 
- The main goal of Reactive Streams is to govern the exchange of stream data across an asynchronous boundary; while ensuring that the receiving side is not forced to buffer arbitrary amounts of data
- In other words, back pressure is an integral part of this model
- Back-pressure controls the rate of events so that a fast producer does not overwhelm its destination.

Before digging deeper into the reactive world, let us understand the difference between blocking and non-blocking request processing.
#### Blocking Request Processing vs Non blocking Request Processing
- In traditional MVC applications, a new servlet thread is created (or obtained from the thread pool) when a request comes to the server. 
- It delegates the request to worker threads for I/O operations such as database access etc. 
- During the time worker threads are busy, the servlet thread (request thread) remains in waiting status, and thus it is blocked. 
- It is also called synchronous request processing 

- In non-blocking or asynchronous request processing, no thread is in waiting state. There is generally only one request thread receiving the request.
- All incoming requests come with an event handler and callback information. 
- Request thread delegates the incoming requests to a thread pool (generally a small number of threads) which delegates the request to its handler function and immediately starts processing other incoming requests from the request thread.
- When the handler function is complete, one thread from the pool collects the response and passes it to the call back function.

---
##### Now, what is reactive programming?
- The term, “reactive,” refers to programming **models that are built around reacting to changes**. 
- It is *built around the publisher-subscriber pattern (observer pattern)*. 
- In the reactive style of programming, we make a request for resources and start performing other things. 
- When the data is available, we get the notification along with data in the callback function. 
- The callback function handles the response as per application/user needs.
### Reactive Streams API
##### Publisher
The publisher emits a sequence of events to subscribers according to the demand received from its subscribers. A publisher can serve multiple subscribers.

```java
public interface Publisher<T> {
  public void subscribe(Subscriber<? super T> s);
}
```

> [!note]
> Spring WebFlux heavily uses two publishers:
> - Mono: Returns 0 or 1 element
> - Flux: Returns 0 to N elements
> 
> A Flux can be endless, meaning that it can keep emitting elements forever. Also it can return a sequence of elements and then send a completion notification when it has returned all of its elements.
##### Subscriber
Receives and processes events emitted by a Publisher.
```java
public interface Subscriber<T> {
  public void onSubscribe(Subscription s);
  public void onNext(T t);
  public void onError(Throwable t);
  public void onComplete();
}
```
##### Subscription
Defines a one-to-one relationship between a Publisher and a Subscriber. It can only be used once by a single Subscriber. 
It is used to both signal desire for data and cancels demand (and allow resource cleanup).
```java
public interface Subscription<T> {
  public void request(long n);
  public void cancel();
}
```
##### Processor
Represents a processing stage consisting of both a Subscriber and a Publisher and obeys both contracts.
```java
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {
}
```
Two popular implementations of reactive streams are `RxJava` (https://github.com/ReactiveX/RxJava) and `Project Reactor` (https://projectreactor.io/).

## WebFlux
- Spring WebFlux is a parallel version of Spring MVC and supports fully non-blocking reactive streams. 
- <mark style="background: #B266FF;">It supports the back pressure concept and uses Netty as the inbuilt server to run reactive applications</mark>
- Spring Web flux uses project reactor as the reactive library.

Spring WebFlux comes in two flavors: functional and annotation-based. 
The annotation-based one is quite close to the Spring MVC model
##### Annotation Based:
```java
@RestController
@RequestMapping("/users")
public class MyRestController {

	private final UserRepository userRepository;

	private final CustomerRepository customerRepository;

	public MyRestController(UserRepository userRepository, CustomerRepository customerRepository) {
		this.userRepository = userRepository;
		this.customerRepository = customerRepository;
	}

	@GetMapping("/{userId}")
	public Mono<User> getUser(@PathVariable Long userId) {
		return this.userRepository.findById(userId);
	}

	@GetMapping("/{userId}/customers")
	public Flux<Customer> getUserCustomers(@PathVariable Long userId) {
		return this.userRepository.findById(userId).flatMapMany(this.customerRepository::findByUser);
	}

	@DeleteMapping("/{userId}")
	public Mono<Void> deleteUser(@PathVariable Long userId) {
		return this.userRepository.deleteById(userId);
	}

}
```
##### Functional based:
- an HTTP request is handled with a `HandlerFunction`: a function that takes `ServerRequest` and returns a delayed `ServerResponse` (i.e. `Mono<ServerResponse>`).
- `HandlerFunction` is the equivalent of the body of a `@RequestMapping`
- Incoming requests are routed to a handler function with a `RouterFunction`: a function that takes `ServerRequest` and returns a delayed `HandlerFunction` (i.e. `Mono<HandlerFunction>`). 
- When the router function matches, a handler function is returned; otherwise an empty Mono.
- `RouterFunction` is the equivalent of a `@RequestMapping` annotation, but with the major difference that router functions provide not just data, but also behavior.
	- `RouterFunctions.route()` provides a router builder that facilitates the creation of routers
```java
@Configuration(proxyBeanMethods = false)
public class MyRoutingConfiguration {

	private static final RequestPredicate ACCEPT_JSON = accept(MediaType.APPLICATION_JSON);

	@Bean
	public RouterFunction<ServerResponse> monoRouterFunction(MyUserHandler userHandler) {
		return route()
				.GET("/{user}", ACCEPT_JSON, userHandler::getUser)
				.GET("/{user}/customers", ACCEPT_JSON, userHandler::getUserCustomers)
				.DELETE("/{user}", ACCEPT_JSON, userHandler::deleteUser)
				.build();
	}
}

@Component
public class MyUserHandler {
	public Mono<ServerResponse> getUser(ServerRequest request) {...}
	public Mono<ServerResponse> getUserCustomers(ServerRequest request) {...}
	public Mono<ServerResponse> deleteUser(ServerRequest request) {...}
}
```

The above methods can contain the following implementation:
```java
Mono<Employee> employeeMono = client.get()
  .uri("/employees/{id}", "1")
  .retrieve()
  .bodyToMono(Employee.class);

employeeMono.subscribe(System.out::println);
```

---
