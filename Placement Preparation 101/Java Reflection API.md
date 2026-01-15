---
tags:
  - placement-preparation
  - java
last-reviewed: 2026-01-15
---

#placement-preparation #java 

##### Core Understanding of Reflection
> [!TLDR] 
> Reflection enables runtime inspection and manipulation of classes
> Inspection includes - discovering class details, methods, fields, constructors
> Manipulation includes - changing access modifier

Without Reflection:
- Dependency Injection would not work
- Annotations would be useless

However, performance becomes slower while using Reflection. 
Hence, reflection trades safety and performance for flexibility.

---
#### Reflection API Basics
Reflection API mainly lives in: `java.lang.reflect` package. Important classes generally used includes:
1. Class
2. Method
3. Field
4. Constructor

##### The Class Object
*`Class` object is used to retrieve metadata of a class at runtime*:
1. Class name
2. Methods
3. Fields
4. Constructors
5. Annotations
6. Interfaces & superclass


> [!info] 
> Metadata of any class is stored in **Metaspace** memory. Hence, the `Class` object is used to access this Metaspace memory area
> 
> High level flow:
> 1. ClassLoader loads the class
> 2. JVM stores metadata in Metaspace
> 3. Class object read this metadata


###### Ways to obtain a `Class` object
1. ClassName.class
	```java
	Class<OrderService> cls = OrderService.class;
	```
	- Class must be known at compile time
	- Ideally, any framework will never use this
	
2. object.getClass()
	```java
	OrderService obj = new OrderService();
	Class<?> cls = obj.getClass();
	```
	- Requires object creation
	- Not useful for dynamic loading
3. Class.forName() ⭐
	```java
	Class<?> cls = Class.forName("com.app.OrderService");
	```
	- Class name comes as String
	- Loaded dynamically i.e., you can pass the string argument at runtime and it will load the class for you
	- Used heavily by frameworks

---

Let's create a class for example (will reuse it going ahead):
```java
public class BaseService {
    protected String serviceType = "BASE_SERVICE";
}

public interface Auditable {
    void audit();
}

public class OrderService extends BaseService implements Auditable {

	private int orderCount = 10;
    public String serviceName = "Order";
    public static String VERSION = "1.0";
    public final String REGION = "IN";
    
    public OrderService() {
        System.out.println("OrderService constructor called");
    }
    
    public void placeOrder(String orderId) {
        System.out.println("Placing order: " + orderId);
    }

    private void calculateTax() {
        System.out.println("Calculating tax");
    }

    @Override
    public void audit() {
        System.out.println("Auditing order service");
    }
}
```

---
##### Creating objects using Reflection
Normally, objects are created using `new`, which requires the class at compile time. Reflection removes this requirement.

Using reflection:
- Class name can come from configuration
- Object is created at runtime

> [!info] 
> Since object are created at runtime, existence of class is not checked at compiletime
> 👉 Reflection shifts errors from compile time to runtime. 

<span style="color:rgb(146, 208, 80)">This is the foundation of Dependency Injection.</span> 


**Normal Way**:
```java
OrderService service = new OrderService();
```

**Reflection Way**:
```java
Class<?> clazz = Class.forName("com.app.OrderService");
Object obj = clazz.getDeclaredConstructor().newInstance();
```

> Casting of `obj` errors may occur at runtime. Frameworks handle this internally to keep your code clean

---

##### Invoking method using reflection
```java
Class<?> clazz = Class.forName("com.app.OrderService");
Object obj = clazz.getDeclaredConstructor().newInstance();

//invoking public method
Method method = clazz.getMethod("placeOrder", String.class);
method.invoke(obj, "ORD-101");

//invoking private method
Method privateMethod = clazz.getDeclaredMethod("calculateTax");
privateMethod.setAccessible(true);
privateMethod.invoke(obj);
```

> [!important] 
> `setAccessible(true)` here or anywhere that I have used below...will always *break encapsulation* 

🟡Compiler does not verify method existence
🟡Errors occur at runtime

---
##### Accessing fields using reflection
```java
//accessing public field
Field publicField = clazz.getField("serviceName");
System.out.println(publicField.get(obj));

//accessing private field
Field privateField = clazz.getDeclaredField("orderCount");
privateField.setAccessible(true);

//set its value as 25...it is similar to how you would set a public field: obj.field = 25
privateField.set(obj, 25);

//then access it the way you would access a public field: obj.field
System.out.println(privateField.get(obj));     


//accessing static field
Field staticField = clazz.getDeclaredField("VERSION");
System.out.println(staticField.get(null));    //passing null, bcoz static field is accessible w/o obj

//accessing final field
Field finalField = clazz.getDeclaredField("REGION");
finalField.setAccessible(true);
System.out.println(finalField.get(obj));
```

##### Reflection at Configuration Level
We want the application to:
- Decide which class to use without changing code
- Control behavior using configuration

**Add class name in .properties file**
```properties
service.class=com.app.OrderService
```

**And then use it...**
```java
String className = props.getProperty("service.class");
Class<?> cls = Class.forName(className);
```

> [!summary] 
> This pattern externalizes class selection to configuration and uses reflection to load and instantiate the class at runtime, enabling loose coupling and forming the basis of Dependency Injection and IoC frameworks like Spring. 

---

#### Reflection in Spring Boot Annotations

> [!hint] 
> Reflection is the engine, annotations are the instructions.  

<span style="color:rgb(0, 176, 240)">Annotations are just metadata</span>. 

When a `@Service` or `@Repository` annotation is added, JVM doesn't do anything. JVM doesn't even scans it when the application starts, Spring manually scans it by calling `ClassLoader` and loads every class (w/o creating objects). 

So Spring does something conceptually like:
```java
Class<?> cls = Class.forName("com.app.OrderService");
```

It doesn't instantly creates object like we saw before using - 
```java
Object obj = cls.getDeclaredConstructor().newInstance();
```

But it only loads the class metadata into memory. Normally, as you saw above, metadata includes constructors, methods, fields, etc. But **in Spring, metadata also includes Annotations**

Hence, as I said earlier - 
> <span style="color:rgb(0, 176, 240)">Annotations are just metadata</span>.

---

Spring now asks the Class object:
```java
if(cls.isAnnotationPresent(Service.class) || cls.isAnnotationPresent(Repository.class)) {
	//if yes, then Spring says, I will manage this class' lifecycle
}
```

This is how Spring finds all the class annotated with different annotations. Once it lists down such classes, it knows which class's lifecycle it has to manage. Now the next question is - How Autowiring works?

###### How does Autowiring works?
When spring needs to create an instance of a service, it looks inside the fields in the class annotated with `@Autowired` and stores it in `Field` object (as we saw earlier)

```java
@Service
public class OrderService {
	@Autowired
	private OrderRepository repository;
}
```

Then, it injects dependency into it using reflection - 
```java
Class<?> clazz = Class.forName("com.app.OrderService");
Field repository = clazz.getDeclaredField("repository");
Object orderServiceObj = clazz.getDeclaredConstructor().newInstance();

Class<?> clazz2 = Class.forName("com.app.OrderRepository");
Object orderRepositoryObj = clazz2.getDeclaredConstructor().newInstance();

repository.set(orderServiceObj, orderRepositoryObj);   //similar to orderServiceObj.field = orderRepositoryObj;
```

> [!important] 
> You have a habit of using `private` for any field with `@Autowired`
> ```java
> @Autowired
> private OrderRepository repository;
> ```
> 
> So what Spring does in such cases is - it temporarily breaks encapsulation by using `setAccessible(true)`


---
###### How AOP works in Spring Boot?

In AOP, you might want to run some logic before/after a method execution. 

But Spring does **NOT** put before/after logic inside your method because Java cannot modify method bodies at runtime
It puts your method inside another object - called a **proxy or dynamic proxy**

A dynamic proxy is an object created at runtime that looks like your service but intercepts method calls and delegates to the real object

---

Now let's go step by step:

Step 1: Scanning of class when Application starts
```java
Class<?> cls = Class.forName("com.app.OrderService");
```

Step 2: Spring inspects each method using reflection:
```java
for (Method m : cls.getDeclaredMethods()) {
    if (m.isAnnotationPresent(Transactional.class)) {
        // interception needed
    }
}
```

Step 3: Spring creates TWO objects
```java
//real object
OrderService target = new OrderService();

//proxy object
OrderService proxy = createProxy(target);    //real object placed inside proxy
```

Inside the proxy:
```
The code before method call is executed
|
Actual method execution
|
The code after method call is executed
```

Similar flow is used when `@Transactional` annotation is detected

---
###### Transactional Annotations

If your class has:
```java
@Transactional
public void placeOrder() {}
```

Spring:
- Detects annotations like `@Transactional` when scanning all classes
- Decides interception is needed 
- Creates a proxy object
- Registers proxy as the bean
- Injects proxy everywhere required

---
> [!summary] 
> Typical framework workflow:
> 1. Scan classpath
> 2. Load classes using `ClassLoader`
> 3. Use Reflection to:
> 4. Check annotations
> 5. Read metadata
> 6. Create objects dynamically
> 7. Inject dependencies
> 8. Apply proxies (AOP)
> 
> Spring uses reflection to scan methods for AOP annotations, records which methods need interception, creates proxy objects during startup, and at runtime the proxy invokes the target method using Method.invoke() while executing before and after advice around it.

---
### 🔥 Top 30 Frequently Asked Interview Questions & Answers
Q 1. What is Reflection API in Java?
Answer:
Reflection API allows a Java program to inspect classes, methods, fi elds, and constructors at runtime and perform operations on them even if they are not known at compile time.

Q 2. Why does Java provide Reflection?
Answer:
Java provides Reflection to support frameworks and libraries that need to work with unknown classes at runtime, such as Spring, Hibernate, and JUnit.

Q 3. Is Reflection used in normal business logic?
Answer:
No. Reflection is mainly used by frameworks. Using Refl ection directly in business logic is discouraged due to performance and maintenance issues.

Q 4. Which package contains Reflection API?
Answer:
The Reflction API is mainly present in the `java.lang.reflect` package.

Q 5. What is the Class object in Refl ection?
Answer:
The Class object represents the runtime metadata of a class. It is the entry point for all reflection operations.

Q 6. How can you get a Class object in Java?
Answer:
There are three ways:
1.`ClassName.class`
2.`object.getClass()`
3.`Class.forName("fullyQualifiedName")`

Q 7. Which way of getting Class object is mostly used by frameworks?
Answer:
Class.forName() is mostly used by frameworks because the class name can be provided dynamically through configuration.

Q 8. Can Reflection create objects without new keyword?
Answer:
Yes. Reflection can create objects using constructors obtained at runtime via `getDeclaredConstructor().newInstance()`

Q 9. How do you invoke a method using Reflection?
Answer:
By obtaining a `Method` object using `getDeclaredMethod()` and invoking it using `method.invoke(object)`.

Q 10. Can Reflection access private fields and methods?
Answer:
Yes. Reflection can access private members by calling `setAccessible(true)`, but this breaks encapsulation and should be used carefully.

Q 11. Does Reflection break encapsulation?
Answer:
Yes. Reflection can bypass access modifiers like private, which is why it is powerful but risky.

Q 12. What is the performance impact of Reflection?
Answer:
Reflection is slower than normal method calls because method resolution happens at runtime and JVM optimizations are limited.

Q 13. Why is Reflection slower than normal method calls?
Answer:
Because Reflection involves runtime lookup, security checks, and dynamic dispatch, which adds overhead.

Q 14. How do frameworks reduce Reflection performance cost?
Answer:
Frameworks cache reflected objects like `Method`, `Field`, and `Constructor` to avoid repeated lookup.

Q 15. What is `setAccessible(true)` used for?
Answer:
It disables Java language access checks, allowing access to private members.

Q 16. What are the security concerns of Reflection?
Answer:
Reflection can expose private data, modify internal state, and bypass security checks, which can lead to vulnerabilities if misused.

Q 17. How does Reflection relate to Dependency Injection?
Answer:
DI frameworks use Reflection to create objects, inject dependencies, and wire components dynamically based on configuration or annotations.

Q 18. How does Spring use Reflection?
Answer:
Spring uses Reflection to:
- Scan classes
- Read annotations
- Create beans
- Inject dependencies
- Apply AOP proxies

Q 19. How does Reflection work with annotations?
Answer:
Reflection reads annotation metadata at runtime to determine behavior such as component scanning, transaction management, and validation.

Q 20. Can Reflection be used to change method behavior?
Answer:
No. Reflection can invoke methods and access fields but cannot change method implementation. Proxies are used for behavior interception.

Q 21. What are dynamic proxies in Java?
Answer:
Dynamic proxies allow runtime creation of proxy objects that intercept method calls, commonly used in logging, security, and transactions.

Q 22. Can Reflection cause memory leaks?
Answer:
Yes. Improper class loading or proxy generation using Refl ection can cause Metaspace memory leaks.

Q 23. What happens if Reflection fails at runtime?
Answer:
It throws runtime exceptions such as `ClassNotFoundException`, `NoSuchMethodException`, or `IllegalAccessException`.

Q 24. Why are Reflection errors hard to debug?
Answer:
Because errors occur at runtime and are not caught during compilation.

Q 25. When should Reflection be avoided?
Answer:
Reflection should be avoided when:
- Classes are known at compile time
- Performance is critical
- Simpler design is possible

Q 26. Can Reflection be used in multithreading?
Answer:
Yes, but reflection objects should be cached and thread safety must be ensured.

Q 27. Is Reflection thread-safe?
Answer:
Reflection API itself is thread-safe, but accessed objects must be handled carefully.

Q 28. What exceptions are commonly thrown by Reflection?
Answer:
- `ClassNotFoundException`
- `NoSuchMethodException`
- `IllegalAccessException`
- `InvocationTargetException`

Q 29. What is the biggest advantage of Reflection?
Answer:
Runtime flexibility and dynamic behavior.

Q 30. What is the biggest disadvantage of Reflection?
Answer:
Performance overhead, loss of type safety, and security risks.