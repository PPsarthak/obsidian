#placement-preparation #java

Memory Management is the process by which the JVM allocates memory to objects, manages
their lifetime, and automatically frees memory that is no longer in use, without requiring the
developer to explicitly deallocate memory.

### How Memory Management Works (Step by Step)
1. JVM starts and creates <span style="color:rgb(240, 81, 105)">memory areas</span> (Heap, Stack, Metaspace, etc.)
2. Objects are created in Heap
3. Local variables and method calls go to Stack
4. JVM tracks which objects are still in use
5. Garbage Collector removes unused objects
6. Freed memory is reused automatically

Example:

Garbage Collection automatically removes objects that are no longer reachable

```java
Student s = new Student();
s = null;
```

- As per OOP, the `new` operator is responsible for actually creating the object by allocating memory and return reference to it
- So initially, on line 1, `s` contains memory reference
- On line 2, `s` is assigned to `null`
- Thus, the object created in memory has no longer any reference (any pointer pointing to that memory location). Hence the object becomes eligible for Garbage Collection

> Additional Pointer: `new` allocates memory in `Heap` and returns a reference to it which itself is stored in `Stack`

### Java Memory Areas
Java is built on different memory areas, each designed for a specific purpose

Main memory areas are:
- Heap
- Stack
- Metaspace
- PC Register
- Native Method Stack

### **Heap Memory**
##### Important points to remember:
- Heap memory is the main area where <mark style="background: #33EE99;">objects and arrays are stored</mark>
- Heap <mark style="background: #33EE99;">allows objects to live beyond method execution</mark>
- Heap memory is <mark style="background: #33EE99;">shared across all threads</mark>
- Managed by Garbage Collector
- Objects live here until they are no longer needed

If Java did not have heap memory:
- Objects would be destroyed when a method ends
- Data sharing between methods would be impossible
- <mark style="background: #FF6699;">OOP would not exist</mark>

Heap ensures that:
- Objects remain available as long as references exist
- Objects can be shared across methods and threads


> [!important] 
>  In the code below:
>  ```java
>  Student s = new Student();
>  ```
>  `Student` object is created in Heap; but reference variable `s` is stored in Stack


> [!faq] 
> In modern Java, the String Constant Pool lives inside the Heap. 
> Even though Strings are often "constant-like," they were moved out of the PermGen/Metaspace area into the Heap to allow the Garbage Collector to manage them better. 

##### Heap Structure
Heap is internally divided into (<span style="color:rgb(240, 81, 105)">BASED ON GENERATIONAL MODEL</span>)- 
- **Young Generation**: Stores newly created objects
- **Old Generation**: Stores long-living objects


###### Young Generation:
- **Eden Space**: 
	- Where almost all new objects are born.
- **Survivor Spaces (S0 & S1)**: 
	- When Eden fills up, a "Minor GC" occurs. Survivors are moved to one of these spaces. 
	- They "age" here before moving to the Old Generation.
	- *The Swap*: During each Minor GC, objects are shuffled between S0 and S1. Each time they survive, their "age" (tenure) increases.

###### Old (Tenured) Generation:
- Holds objects that have survived enough rounds in the Survivor spaces (reached a certain "threshold"; usually 15).
- Usually larger than the Young Generation. 
- A "Major GC" (or Full GC) happens here, which is more "expensive" and can cause application pauses.
- This area holds long-lived data (like <span style="color:rgb(240, 81, 105)">Spring Beans, Caches, or Singleton instances</span>).

### **Stack Memory**
##### Important Points to remember:
- Stack memory is used by JVM to track method calls
- Each thread gets its own stack to store - Local variables, method parameters, method call information (which line called it or return address)

If Java did not have stack memory:
- Method execution would not be possible
- Multi-threading would not be isolated; thread safety would break
- Recursion would be impossible

> [!info] 
>  StackOverflowError is directly related to stack memory management
> If too many method calls are added to stack:
> ```java
> void test() {
>     test();
> }
> ```
> This would cause the stack memory to exceed its limit and JVM would need to throw StackOverflowError


### **Metaspace**
##### Important points to remember:
- Metaspace stores class-level information, not objects.
- Metaspace is Native Memory (not part of the JVM Heap) and is auto-resizable.
- Class information contains:
	- Class name
	- Method definition
	- Field information
	- Inheritance details


> [!important] 
> Static Variables (since Java 8) now reside in the Heap (associated with the Class object), not Metaspace. 
> 
> But still it is crazy to think that - static variables which are generally associated with the class are not stored in Metaspace but in Heap


If Java did not have metaspace memory:
- JVM wouldn't know what methods & fields a class has
- Inheritance would be impossible
- Java Reflection would fail
- Spring Framework would not work


> [!important] 
> It is important to note that Metaspace replaced "PermGen" (Permanent Generation) starting with Java 8
> 
> Key Distinction: 
> - PermGen was part of the Heap and had a fixed max size. 
> - Metaspace is part of Native Memory (RAM provided by the OS), not the JVM’s pre-allocated memory.
> - This means it can grow automatically based on what the OS allows, reducing `OutOfMemoryError: PermGen space` issues. 

### **Program Counter Register**
Each thread has a PC (Program Counter) Register.
It <mark style="background: #33EE99;">stores the address of the current instruction being executed</mark>

Why needed?
- JVM executes bytecode step by step
- PC Register helps JVM resume execution correctly

If Java did not have PC register:
- Thread execution would break
- JVM would not know what to execute next

### **Native Method Stack**
This memory area <mark style="background: #FF6688A6;">supports native (non-Java) code execution</mark>
Used when:
- Java interacts with OS
- JVM calls C/C++ code internally

This is often called the <mark style="background: #FF6688A6;">JNI (Java Native Interface) area</mark>.

Example:
- Opening/operating on a file

> This stack ensures Java can work with system-level operations safely

---

#### Summary Table / Cheat Sheet

| Memory Area     | Stores                          | Lifecycle                     | Thread-Safe?            |
| --------------- | ------------------------------- | ----------------------------- | ----------------------- |
| **Stack**       | Local variables & Method frames | Until method ends             | Yes (Private to thread) |
| **Heap**        | Objects & Arrays                | Until no reference exists     | No (Shared)             |
| **Metaspace**   | Class definitions & Metadata    | Until ClassLoader is unloaded | No (Shared)             |
| **PC Register** | Current instruction address     | Until thread dies             | Yes (Private to thread) |


### Types of Garbage Collection
- Minor GC → Cleans Young Generation (fast, frequent)
- Major GC → Cleans Old Generation (slower)
- Full GC → Cleans entire Heap (performance impact)

#### Reachability (GC roots concept)
Objects are considered alive if reachable from:
- Local variables
- Static fields
- Active threads
- JNI references

#### How Garbage Collection Works (The "Mark and Sweep" Algorithm)
```java
Student s = new Student();
s = null;
```

Here is what the JVM actually does behind the scenes:
1. **Marking**: The GC traverses the *"Object Tree"* starting from GC Roots (local variables in the Stack, static variables). It "marks" every object it can reach as Live.
2. **Sweeping**: It identifies the "unmarked" objects (like Student object with no reference) and clears them from memory.
3. **Compacting** (Optional): To prevent memory fragmentation (holes in memory), the GC moves the remaining live objects together to create a solid block of free space.

#### Common JVM Memory Flags
When you run a Java application (e.g., `java -jar app.jar`), you can pass these arguments to prevent `OutOfMemoryError`


| Flag                     | Purpose                            | Memory Area Affected |
| ------------------------ | ---------------------------------- | -------------------- |
| **-Xms**                 | Sets the Initial Heap size         | Heap                 |
| **-Xmx**                 | Sets the Maximum Heap size         | Heap                 |
| **-Xss**                 | Sets the size of each Thread Stack | Stack                |
| **-XX:MetaspaceSize**    | Sets the initial size of Metaspace | Metaspace            |
| **-XX:MaxMetaspaceSize** | Sets the upper limit for Metaspace | Metaspace            |
| **-XX:GC**               | Sets the GC Engine to use          |                      |

> [!note]
> `-Xmx` and `-XX:MaxMetaspaceSize` are used to set the upper limit for Heap and Metaspace respectively. But there is no provision to set upper limit for Stack
> `-Xss` sets the size of each thread. That itself is the max because total stack memory is indirectly limited by OS memory and thread count

##### Example of Commands

| Memory        | Command                                       | Explanation                                           |
| ------------- | --------------------------------------------- | ----------------------------------------------------- |
| Set Heap      | `java -Xms512m -Xmx2g -jar app.jar`           | Starts at 512MB, grows to 2GB                         |
| Set Stack     | `java -Xss512k -jar app.jar`                  | Each thread gets 512KB instead of the default 1MB     |
| Set Metaspace | `java -XX:MaxMetaspaceSize=256m -jar app.jar` | Prevents class metadata from consuming all system RAM |

### Modern Garbage Collectors

The JVM has different engines of Garbage Collection:
1. Serial GC:
	- `-XX:+UseSerialGC` 
	- <span style="color:rgb(240, 81, 105)">Uses a single thread for GC</span>
	- When it’s time to clean up, <mark style="background: #FF6688A6;">it freezes the entire application and uses one CPU core to perform the Mark-Sweep-Compact process</mark>
	- <span style="color:rgb(240, 81, 105)">Good for tiny applications or microservices running in resource-constrained Docker containers</span> (like a "sidecar" that just handles logs)
	- If your heap is large, the pause time will be very long, because only one thread is used
	
2. G1 GC
	- `-XX:+UseG1GC`
	- The <span style="color:rgb(240, 81, 105)">default since Java 9</span>.
	- <mark style="background: #FF6688A6;">Instead of treating the Heap as one giant Young and one giant Old generation, G1 splits the heap into ~2,000 small, equal-sized regions</mark>
	- It tracks how much "reclaimable" space is in each region. When it needs to free memory, <span style="color:rgb(240, 81, 105)">it picks the regions that are mostly full of garbage first</span> hence the name
	- It’s designed to avoid long "Stop-the-World" pauses.
	
> You can actually tell G1 your "Max Pause Time Goal" (e.g., `-XX:MaxGCPauseMillis=200`). G1 will try its best to only clean as many regions as it can within that 200ms window.


3. ZGC / Shenandoah
	- ZGC: `-XX:+UseZGC`; Shenandoah: `-XX:+UseShenandoahGC`
	- The newest "Ultra-low latency" collectors. (JDK 11+)
	- They do almost all the work while the application is still running, keeping pauses under 1 millisecond.
	- Concurrent - meaning <span style="color:rgb(240, 81, 105)">they clean the memory while your application threads are still running</span>.
	- Downside - CPU "overhead" because the GC threads are constantly running alongside your app threads.

> [!info] 
> ZGC and Shenandoah are different models but use the same approach to acheive low latency GC
> ZGC → Oracle’s low-latency GC
> Shenandoah → Red Hat’s low-latency GC
> Same goal, different designs


> [!important] **"Most objects die young" - Generational Model**
> 
> The Generational Model is a memory management strategy based on one key observation:
> **Most objects die young**
> 
> Instead of treating all objects the same, the JVM groups objects by age and applies different GC strategies to each group. This is the actual strategy where Young Generation and Old Generation is introduced
>
> Historically, ZGC was non-generational (single logical heap). Everything was treated equally.
> `-XX:+ZGenerational` enables Generational mode on top of ZGC (Java 21+)
> So, the entire command would be - `-XX:+UseZGC -XX:+ZGenerational`

### Monitoring Memory with Spring Boot Actuator

Add the dependency:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Add properties
```properties
# Expose all actuator endpoints (use carefully in production!)
management.endpoints.web.exposure.include=metrics,health,info
```

See Total Memory Usage: 
- `GET http://localhost:8080/actuator/metrics/jvm.memory.used`
- This gives you the current bytes used across all areas.

See Specific Areas (Heap/Metaspace): 
- `GET http://localhost:8080/actuator/metrics/jvm.memory.used?tag=area:heap `
- `GET http://localhost:8080/actuator/metrics/jvm.memory.used?tag=area:nonheap`

Check Garbage Collection: 
- `GET http://localhost:8080/actuator/metrics/jvm.gc.pause`
- Shows you how much time the JVM is spending "paused" for GC
