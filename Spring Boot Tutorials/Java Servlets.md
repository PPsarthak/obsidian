---
status: In Progress
last-reviewed: 2025-08-30
tags:
  - spring-boot
  - placement-preparation
---
#spring-boot  #placement-preparation #servlets

### What is a Servlet?
- A Servlet is a Java class that runs on a web server (inside a Servlet container like Apache Tomcat, Jetty, JBoss, WebLogic, etc.) and handles HTTP requests/responses.
- They are the core for a simple client-server web application
- Servlets are part of the Java EE / Jakarta EE specification under the `javax.servlet` (or `jakarta.servlet` in newer versions) package.
#### How do Servlets work?
When you type a URL in your browser:
- Browser sends an HTTP request - E.g., `GET /myapp/hello`
- Web server (Tomcat) receives it and checks its deployment descriptor (mapping of URLs to servlets).
- The server creates or reuses a Servlet instance *(reuse the same object maybe??)* and calls its methods to process the request, and generates a response.
- Response is sent back to the browser (HTML, JSP, JSON, XML, etc.)

Now this all is done by a servlet container (Tomcat). It also -
- Manages servlet lifecycle.
- Provides request and response objects (`HttpServletRequest`, `HttpServletResponse`)
#### Creating a Servlet
1. Create a class extending `HttpServlet`.
2. Override methods like `doGet()` or `doPost()`.
3. Compile and package inside a web application (WAR).
4. Deploy WAR to a Servlet container (Tomcat, Jetty).
5. Map URL pattern to your servlet (via web.xml or annotations)
##### Example - 
###### Creating a custom servlet - extending `HttpServlet`
```Java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h1>Hello, World from Servlet!</h1>");
        out.println("</body></html>");
    }
}
```

**Request & Response Objects**:
`HttpServletRequest`:
- Represents client request.
- Methods:
	- `getParameter("name")` → form data.
	- `getMethod()` → GET/POST.
	- `getHeader("User-Agent")` → header info.
	- `getSession()` → session tracking

`HttpServletResponse`:
- Represents server response.
- Methods:
	- `setContentType("text/html")`
	- `getWriter() `→ output HTML/text
	- `sendRedirect("url")` → redirect client
	- `setStatus(404)` → set status codes.
###### Deployment Configuration
Now you can either add a servlet mapping in `WEB-INF/web.xml` -
```xml
<web-app>
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.example.HelloServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

or use an annotation `@WebServlet()` - 
```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet { ... }
```
#### Servlet Lifecycle
Servlet Lifecycle is managed by the Servlet Container.
1. Loading & Instantiation
	- Container loads servlet class into memory (first request or preloaded).
	- Creates only one instance of servlet.
2. Initialization `init()`
	- Called once 
	- Used for setup tasks (DB connections, reading configs)
3. Request Handling `service()`
	- For every client request, the container spawns a new thread.
	- Calls `service()` which dispatches to `doGet()`, `doPost()`, etc.
4. Destruction `destroy()`
	- Called once when container shuts down.
	- Used for cleanup (closing DB connections, releasing resources)

#### A Complete Example:
Let's assume we have created a Maven Project and let's assume I want to return a `.jsp` page saying "hello" when someone hits GET /hello
Here would be our file structure -
```
MyWebApp/
 ├── src/main/java
 │    └── com/example/HelloServlet.java
 ├── src/main/webapp
 │    ├── hello.jsp
 │    └── WEB-INF/
 │         └── web.xml
```

Here is our `HelloServlet` class
```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	    request.setAttribute("message", "Hello from the Servlet!");
        RequestDispatcher dispatcher = request.getRequestDispatcher("hello.jsp");
        dispatcher.forward(request, response); // Forward to JSP
    }
}
```

And here is our `hello.jsp`
```jsp
 <%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head>
    <title>Hello JSP</title>
</head>
<body>
    <h1><%= request.getAttribute("message") %></h1>
    %% or simply add a h1 to pring hello %%
</body>
</html>
```

To start deploying, package the project into a `.war` file and add it to `webapps` folder and done!