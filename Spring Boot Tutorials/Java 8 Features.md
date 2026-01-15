---
status: Pending
---
#spring-boot #placement-preparation 

### 📌 Concise List of Java 8 Features
1. Lambda Expressions
2. Functional Interfaces (with `@FunctionalInterface`)
3. Method References
4. Streams API
5. Default Methods in Interfaces
6. Static Methods in Interfaces
7. Optional Class
8. New Date & Time API (java.time)
9. Nashorn JavaScript Engine
10. CompletableFuture & Concurrency Updates
11. Collectors & Enhanced Collections API
12. Parallel Streams
13. New Base64 Encoding/Decoding

![[Java 8 CheatSheet.png|950]]

---
### Lambdas
- Lambda is basically a shorthand for **writing anonymous functions**
- Syntax:
	```
	(parameters) -> expression
	(parameters) -> { statements }
	```

Example:
```java
// Without lambda (old way)
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello World");
    }
};

// With lambda (Java 8)
Runnable r2 = () -> System.out.println("Hello World");
```

---
### Streams API
Before Java 8, if you wanted to filter a list, map it, and then sort it, you had to write nested loops and conditionals. Streams provides a functional and declarative way to process collections.
> Unlike collections, streams do not store elements. They are essentially wrappers around a data source that enable operations to be performed on the data without modifying the original source.

Functional Operations:
Streams support a variety of functional-style operations, including:
- Intermediate Operations: These operations transform a stream into another stream. Examples include `filter(), map(), sorted(), and distinct().` They are lazy, meaning they are not executed until a terminal operation is invoked.
- Terminal Operations: These operations produce a result or a side-effect, consuming the stream. Examples include `forEach(), collect(), reduce(), count(), min(), max(), and findFirst().` 