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
###### Creating objects using Reflection
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

> Casting errors may occur at runtime. Frameworks handle this internally to keep your code clean

---

###### Invoking method using reflection
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
###### Accessing fields using reflection
```java
//accessing public field
Field publicField = clazz.getField("serviceName");
System.out.println(publicField.get(obj));

//accessing private field
Field privateField = clazz.getDeclaredField("orderCount");
privateField.setAccessible(true);
System.out.println(privateField.get(obj));

//accessing static field
Field staticField = clazz.getDeclaredField("VERSION");
System.out.println(staticField.get(null));    //passing null, bcoz static field is accessible w/o obj

//accessing final field
Field finalField = clazz.getDeclaredField("REGION");
finalField.setAccessible(true);
System.out.println(finalField.get(obj));
```

###### Reflection at Configuration Level
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
