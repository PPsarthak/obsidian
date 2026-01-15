#system-design-v2 #design-patterns  #creational-design-pattern

## Glossary
- Ensures only 1 instance of the class exists and also provides a single point of access to it
- This design pattern is used when the goal is to block the creation of new instances and keep a single point of creation. 
- This point of creation will also have the logic to allow only 1 instance. 

#### Key points
- First of all, we need to **make the constructor of the singleton class private**. This way the constructor is not accessible from outside the class and hence object creation is restricted to within the class
- Now to actually create the 1 single object, we can define a method that calls this private constructor. Let's call this method as `getInstance()` and **it is always supposed to be public and static (associated with the class and not the object)**
- The code inside will create a private static instance of the class (private because no access from outside and static because again associated with class level)

> [!danger] Important Note
>I will go from the best approach to the worst approach. Usually I have seen people explain 1 approach and then mention the flaws. 
>This has cluttered my mind
>Hence, I will have the best/optimal approach first

#### Bill Pugh Singleton (Inner static class)
Firstly, a few things about JVM class loading...
> [!note] JVM Class loading
> Java guarantees that class initialization is thread-safe. So when a class is first loaded into memory (called for the first time), it's initialization will always be thread-safe
> Also any `static final` fields in the class will also be initialized then and there (because again it will at class level)
> Therefore, there is no need of `synchronized` or `volatile` to make them thread-safe

The core idea behind the Bill Pugh Singleton is to leverage this Java Virtual Machine's guarantee about class initialization.

```java
public class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

Inside the Singleton class, there's a private static nested class (often named `Holder` or similar). This inner class holds the actual instance of the Singleton.

<mark style="background: #33EE99;">The `Holder` inner class is not loaded by the JVM until it is explicitly referenced. </mark>This means the INSTANCE of Bill Pugh Singleton is only created when `getInstance() `is called for the very first time. 

The `classloader` ensures that `Holder.INSTANCE` is <mark style="background: #33EE99;">initialized only once and only when needed.</mark>

#### Double Checked Locking
We use double checks - first checking if instance is initialized or not; and the next check inside a `synchronized` block for thread safety

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

Cons - Requires `volatile` keyword to prevent instruction reordering

> [!info] Instruction Reordering
> Modern JVMs and CPUs are allowed to reorder instructions for performance—as long as the final result is the same in a single-threaded execution.
> But in multi-threaded environments, this can cause problems if a thread sees a partially constructed object.
> Here's more- 
> An object is said to be completed when memory is allocated AND when that new instance is assigned to memory address
> So say the constructor has 3 steps - 
> 1. Allocate memory
> 2. Initialize the object fields/data
> 3. Assign the reference to `instance`
> 
> and now imagine JVM reorders into: step 1 -> 3 -> 2
> Now at step 3(which is actually number 2 in the reordering), the object will be marked completed, although the fields are not assigned yet.
> So say at this moment, another thread comes in `getInstance()`, the instance will not be null and then return this partially completed object
> So ultimately *what volatile does is avoid the reordering which can lead to performance issue*

#### Eager Initialization
You already create an instance before-hand and then whenever `getInstance()` is called, you return that instance.

```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

Pros - Straightforward and no issues in multi-threading
Cons - Object may be created even though it might never be requested. 