---
status: Done
last-reviewed: 2025-09-15
tags:
  - spring-boot
  - lombok
---
#spring-boot #lombok

Project Lombok is a Java library that significantly reduces boilerplate code in Spring Boot applications
It works by plugging into our build process and auto-generating Java bytecode into our .class files according to some project annotations we introduce in our code. 

---
#### Benefits of using Lombok in Spring Boot:
1. Reduced boilerplate code:
	- Eliminates the need to manually write repetitive code for data classes, leading to cleaner and more concise code.
2. Improved readability:
	- Focuses on the essential logic of the application by removing distracting boilerplate.
3. Faster development:
	- Saves time and effort by automating code generation.
4. Easier maintenance:
	- Changes to data fields automatically reflect in generated methods, reducing potential errors.

#### Key Lombok Annotations commonly used in Spring Boot:
`@Getter` and `@Setter`: Generate getter and setter methods for fields.
`@NoArgsConstructor`: Generates a constructor with no arguments.
`@AllArgsConstructor`: Generates a constructor with all fields as arguments.
`@RequiredArgsConstructor`: Generates a constructor with required (final) fields as parameters.
`@Data`: A convenient shortcut that combines `@Getter`, `@Setter`, `@RequiredArgsConstructor`, `@EqualsAndHashCode`, and `@ToString`.
`@Builder`: Enables the builder pattern for creating objects, offering more readable and flexible object construction.
`@Slf4j` generates a logger for the class, allowing you to log messages without manually creating a Logger instance.

> [!tip]
> You can control access of getters and setters too
> ```java
> @Getter(AccessLevel.PUBLIC)
> @Setter(AccessLevel.PRIVATE)
> public class User {
>    private String firstName;
>    private String lastName;
> }
> ```
> 
> You can also exclude certain fields from the `toString()`
> ```java
> @ToString(exclude = "password")
> public class User {
>     private String firstName;
>     private String lastName;
>     private String password;
> }
> ```

### Additional Annotations
#### @Value
The @Value annotation is used for immutable classes. 
It is similar to @Data, but all fields are marked as private final and no setters are generated. It is useful for creating immutable DTOs
```java
@Value
public class UserDTO {
    private String firstName;
    private String lastName;
    private String email;
}
```
- Lombok generates a constructor that takes all fields as parameters.
- All fields are final and private, making the class immutable.
- No setters are generated, ensuring that the object cannot be modified after creation.
#### @Singular
The @Singular annotation is used with @Builder to allow for adding elements to collections in a builder pattern. It can be applied to collections like List, Set, and Map.
```java
@Builder
public class Team {
    private String name;
    @Singular
    private List<String> members;
}

Team team = Team.builder()
			.name("Development Team")
			.member("John")
			.member("Jane")
			.build();
```

#### @UtilityClass
No instances of it can exist, and all its members are static. For example, `java.lang.Math` and `java.util.Collections` are well known utility classes. This annotation automatically turns the annotated class into one.

A utility class cannot be instantiated. By marking your class with `@UtilityClass`, lombok will automatically generate a private constructor that throws an exception, flags as error any explicit constructors you add, and marks the class final. If the class is an inner class, the class is also marked static.

All members of a utility class are automatically marked as static. Even fields and inner classes.

**Potential issues and considerations**
- Experimental feature: The annotation is still experimental, and the Lombok team has debated its necessity and potential complexities. 
- Static import limitations: There are known issues with using import static with classes annotated with @UtilityClass, often resulting in compilation errors. 