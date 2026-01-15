---
status: Done
last-reviewed: 2025-08-27
tags:
  - spring-boot
  - servlets
  - filters
  - interceptors
---
#spring-boot #placement-preparation #servlets #filters #interceptors
### Flow in Summary
![[Servlet, Filter, Interceptor in Java.png|650]]
```
Client 
↓
Servlet Container (Tomcat - which contains the entire application) 
↓
Filters (provided by Tomcat)
↓
	DispatcherServlet/Front Controller (provided by Spring - handles only @Controller)
		↓
		Interceptors (also provided by Spring)
		↓
		Controller (also provided by Spring)
		↓
		Service → Repository → DB
		
	Your Web Servlet (@WebServlet bypasses DispatcherServlet, no Interceptors)
```
### What is a Servlet?
- Servlet is a Java class that handles requests, processes them and reply back with a response
- Servlets are under the control of another Java application called a Servlet Container. When an application running in a web server receives a request, the Server hands the request to the Servlet Container – which in turn passes it to the target Servlet
- Servlet declares three essential methods for the life cycle of a servlet — *`init(), service(), and destroy()`*
- The Container calls the *`service()`* method to handle requests coming from the client, interprets the HTTP request type (GET, POST, PUT, DELETE, etc.) and calls *`doGet, doPost, doPut, doDelete`* methods as appropriate.

> [!Note] Servlet vs Controller
> Servlet runs directly inside the servlet container. Controllers are a higher-level abstraction built on top of servlets
> 1 More important point of difference added here -> [[Servlet, Filter and Interceptor in Spring Boot#Filters vs Interceptors]]
#### What is a Servlet Container?
- Servlets run in a servlet container which handles the networking side (e.g. parsing an HTTP request, connection handling etc.). 
- It manages servlets (creation, initialization, destruction)
- Tomcat is the most popular one. Others include Jetty, JBoss, etc.
> 👉 Without the container, your servlet code won’t run - it’s the runtime environment for servlets.

#### Front Controller Pattern
The Front Controller is a design pattern where one central servlet handles all incoming requests, instead of having many servlets directly mapped.
###### Why?
- Centralizes request handling (logging, authentication, error handling, etc.).
- Then, it dispatches to the right business logic / controller.
> 👉 Example: Spring MVC uses **`DispatcherServlet`** as a front controller

**Example**
```java
//all servlets needs to be annotated with @WebServlet and must extend HttpServlet

@WebServlet("/*") //All requests go to FrontControllerServlet
public class FrontControllerServlet extends HttpServlet {  
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        String path = req.getPathInfo();
        if ("/login".equals(path)) {
            new LoginController().process(req, resp);
        } else if ("/register".equals(path)) {
            new RegisterController().process(req, resp);
        } else {
            resp.sendError(HttpServletResponse.SC_NOT_FOUND);
        }
    }
}
```
### What is a Filter?
- A filter sits before and/or after your servlet and allows you to manipulate the request or response.
- It is a Java class which is executed by the servlet container for each incoming HTTP request and for each HTTP response.
- If you have multiple custom filters in your application, you can define the order with `@Order` annotation.
##### Methods
- `init(FilterConfig config)` - This is invoked only once. It is used to initialize the filter.
- `doFilter(HttpServletRequest request,HttpServletResponse response, FilterChain chain) `- This method is invoked every time a user send a request to any resource, to which the filter is mapped. It is used to perform filtering tasks
- `destroy()` - This is invoked only once when filter is taken out of the service
##### What Filters Can Do
- Authentication & Authorization (check JWT, API keys).
- Logging & auditing.
- Compressing or modifying responses.
- CORS handling.
- Request wrapping (e.g., reading body multiple times)
### What is an Interceptor
Spring Interceptors are similar to Servlet Filters. An interceptor just allows custom pre-processing with the option of prohibiting the execution of the handler itself, and custom post-processing, having access to Spring Context.
##### Methods
1. **`preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)`**- This is used to perform operations before sending the request to the controller. This method should return true to return the response to the client.
2. **`postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)`** - This is used to perform operations before sending the response to the client.
3. **`afterCompletion(HttpServletRequest request, HttpServletResponse response,Object handler, Exception exception)`** - This is used to perform operations after completing the request and response.

![[Filters vs Interceptors in Spring MVC.png|850]]
###### Example:
```java
@Component
public class MyInterceptor implements HandlerInterceptor {

    // Runs before controller method
    @Override
    public boolean preHandle(HttpServletRequest request,HttpServletResponse response,
				 Object handler) throws Exception {
        System.out.println("PreHandle: Before controller - " + request.getRequestURI());
        return true; // if false, request stops here
    }

    // Runs after controller method, but before view rendering
    @Override
    public void postHandle(HttpServletRequest request,HttpServletResponse response,
			   Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("PostHandle: After controller, before view render");
    }

    // Runs after the entire request is complete
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
				Object handler,Exception ex) throws Exception {
        System.out.println("AfterCompletion: After view render");
    }
}
```
##### There are 2 ways to do the same thing...When to implement which ?
1. **`HandlerInterceptor`** : `HandlerInterceptor` instances are executed as part of the request handling inside the `DispatcherServlet` (which implements `javax.servlet.Servlet`).
2. **`HandlerInterceptorAdapter`**: If you would like to provide a custom implementation and only care for a few of their methods (if you do not want to create empty methods that requires overriding), it is better to implement an adapter.

### Filters vs Interceptors
> [!Tip]+ Analogy
> I am assuming the analogy of a front door and receptionist to understand the filter. 
> So in an office, the front door would be like a filter. Anyone, who is entering the building will go through it. 
> Now let's say my office is on 2nd floor. The interceptor would be the 2nd floor receptionist, who will cater only those who come on 2nd floor. 
> There might be some who enter the front door, and may go to 1st floor or 3rd floor. These are the static resources. Sometimes, `/css/style.css or /images/logo.png or an endpoint which is not even mapped to a controller` are requested and Spring Boot serves these directly from `/static or /public or throws a WhiteLabelError Page` without involving `DispatcherServlet`
> > [!hint]- Servlet vs Container
> > If you have all endpoints mapped to controllers, except 1 which is handled by a `WebServlet` then it will pass through filter but not enter 2nd floor (it will also not even enter Dispatcher Servlet.
> > So it will be like a 1st or 3rd floor visitor
> > (think of some analogy here for Dispatcher Servlet which comes between front gate and office receptionist - maybe like a 2nd floor CCTV camera))
> 
> This way always remember that - **Interceptors will only execute after Filters and filters sees all and interceptor only sees controller mapped requests**

- <mark style="background: #33EE99;">Filter is related to the Servlet API and HandlerIntercepter is a Spring specific concept</mark>
- **Filters can wrap/replace the `HttpServletRequest`** (or simply say the incoming request - they often use `HttpServletRequestWrapper` to create a modified request). Interceptor can read request info (headers, params, body) but **interceptors cannot replace the request object because the `DispatcherServlet` has already parsed it**. At most, they can add attributes to `request.setAttribute()` (which the controller can later read)
- Similarly, filters can wrap/replace the `HttpServletResponse`. (Again, they'd use `HttpServletResponseWrapper`). Interceptors cannot replace the response object. They can only modify the Spring MVC `ModelAndView` (data passed to the view before rendering -useful if you want to inject extra model attributes or tweak view logic)

> The above ones were the most important one. The below ones are less important to understand -

- Fine-grained pre-processing tasks are suitable for HandlerInterceptors (authorization checks, etc.). Because you have access to the actual target “handler”. You can even check if the handler method has a specific annotation.
- Content handling related or generic flows are well-suited for Filters (such as multipart forms, zip compression, image handling, logging requests, authentication etc.)
- Interceptor’s `postHandle()` method will allow you to add more model objects to the view but you can not change the `HttpServletResponse` since it's already committed.
- Filter’s `doFilter()` method is much more versatile than Interceptor’s `postHandle()`. You can change the request or response and pass it to the chain or even block the request processing.

| **Feature** | **Filter**                                      | **Interceptor**                                        |
| ----------- | ----------------------------------------------- | ------------------------------------------------------ |
| Belongs to  | Servlet API                                     | Spring MVC                                             |
| Runs        | Before/after servlet (all requests)             | Before/after controller (Spring-handled requests only) |
| Scope       | Global (all resources: static, JSP, REST, etc.) | Controller layer (REST endpoints, MVC controllers)     |
| API         | `javax.servlet.Filter`                          | `HandlerInterceptor`                                   |
| Control     | Can wrap request/response                       | Can modify model/view                                  |
| Use cases   | Auth, CORS, logging, compression                | Auth, logging, adding model attributes                 |
