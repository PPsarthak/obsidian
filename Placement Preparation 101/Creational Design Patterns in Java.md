#placement-preparation #design-patterns

## Index
- Creational Design Patterns [[#Creational Design Patterns]]
- Prototype Design Pattern [[#Prototype Design Pattern]]
- Singleton Design Pattern [[#Singleton Design Pattern]]
- Builder Design Pattern [[#Builder Design Pattern]]
- Factory Method Design Pattern [[#Factory Method Design Pattern]]
- Abstract Factory Method Design Pattern [[#Abstract Factory Design Pattern]]
---
#### Creational Design Patterns
<mark style="background: #FFB86CA6;">What</mark> - Creational Design Patterns are a way of designing the flow of creation of objects.
<mark style="background: #FFB86CA6;">Need</mark> - In production-level applications, it is important to maintains some constraints for objects due to the purpose they achieve - Logger, DB connections, etc. Hence, to satisfy these constraints, object creation needs to be perfect

> [!quote]
> CDP introduces different design flows for creation of objects having some constraints 

---
#### Prototype Design Pattern
 > [!quote]
> Creates new objects by copying an existing object (c/a Prototype), rather than creating from scratch when creating a new object is costly/complex

So, the need is pretty clear - object creation is complex or costly hence, the need of Prototype Pattern. But how to achieve it?

Say we have to duplicate an object of class `Car`(let's call it `carA`)
So ideally what we do is use the `new`keyword and create another object (call it `carB`) and set the fields using the same value as in `carA`. But as discussed, creating new object is complex, maybe because it has a lot of fields (like 100s of them for example)

Also, by now you must have become that good that you won't write the code to do it in main class or wherever it is required - you will write it somewhere within the class itself. The reason you would do this is -
1. No need to rewrite the code again and again wherever it is required
2. And the most important one, if the class has some private fields which are not accessible outside the class you won't be able to set them from outside

There is one more niche problem with it - though it is not niche at production level.
The problem is say our `Car` implements `Vehicle` class/interface and there is another class `Bus` which also implements `Vehicle`

```Java
public void doSomething(Vehicle vehicle){
	//Create the duplicate inside this method
}
```
Now if the main method or wherever we are creating this object is using `Vehicle` as the reference, then how would be able to determine which object to create at runtime?

##### Prototype Pattern
Generally, the idea is to create an interface (c/a Prototype) which has 1 method - `clone()`
This class sits at the top of the hierarchy chain of `Vehicle`, `Car` and `Bus` - 

![[Prototype CDP in Java.png|450]]

<mark style="background: #FFB8EBA6;">The concrete class implements the `clone()` and includes the logic to automatically create clone of the same object.</mark>
So now you can call `carA.clone()` to create clone of `carA`

> [!summary]+ Deep Copy vs Shallow Copy
> Say the `Car` has a field - `private List<String> features;`
> Now there are 2 ways to implement the `clone()` - 
> ```Java
> @Override
> public Car clone(){                 //SHALLOW COPY
> 	return new Car(this.features);
> }
> //OR
> @Override
> public Car clone(){                 //DEEP COPY
> 	return new Car(new ArrayList<>(this.features));
> }
> ```
> 
> Difference is if you create `carB` as a shallow copy, then the same features list will be added in `carB`. So now if the features in `carA` change, so will features in `carB`. In deep copy, a new list is created and hence, changes in `carA` will not be reflected in `carB` 
###### Using copy constructor
In the above code, what we did is call the parameterized constructor and passed in all the fields that we want to set  - `return new Car(a,b,c...z);`
And you see this is the problem, what if there are more than 100 fields or so, will you write all the fields in the constructor?

The solution is using the concept of Copy Constructor from OOP -
```Java
public Car(Car other) {       //Copy Constructor
	// Choose between shallow or deep copy:
	this.features = new ArrayList<>(other.features);
}

//And your clone method - 
@Override
public Car clone() {
	return new Car(this);  // clean and readable
}
```

![[Prototype Design Pattern UML Class Diagram.png|750]]
##### Prototype Registry

	Centralized storage (usually a map/dictionary) where you store pre-configured instances, and then clone them on demand

Say you have multiple Car objects - `carA`, `carB`, `carC` with little modifications in some fields. You may put  `carB` in this registry and then always the clone of `carB` will be returned.

<mark style="background: #ADCCFFA6;">Think of it as a registry/hashmap that holds the perfect object of the class and uses it to make clones again and again</mark>

To implement it, you can simply add a class `VehicleRegistry`-
```Java
class VehicleRegistry {
    private Map<String, Vehicle> prototypes = new HashMap<>();
	
    public void register(String key, Vehicle prototype) {
        prototypes.put(key, prototype);
    }

    public Vehicle getClone(String key) {
        Vehicle prototype = prototypes.get(key);
        return (prototype != null) ? prototype.clone() : null;
    }
}

//And while cloning
public void doSomething(){
	VehicleRegistry registry = new VehicleRegistry();

	Car baseCar = new Car("Model S", List.of("GPS", "Sunroof"));
	registry.register("electric", baseCar);

	Car car1 = (Car) registry.getClone("electric");  //use registry to clone objects
	car1.getFeatures().add("Heated Seats");

	System.out.println(car1);  // Modified clone
}
```

###### Reasons why this approach of cloning is good
- It follows Open-Closed Principle - If you implement another class `Truck` in future, you won't need to modify any existing code
- The code automatically figures out which instance to create when `Vehicle` is used as reference - 
```Java
public void doSomething(List<Vehicle> list){
	List<Vehicle> cloneList = new ArrayList<>();
	
	for(Vehicle vehicle : list){
		cloneList.add(vehicle.clone());
	}
}
```
---
#### Singleton Design Pattern
>[!quote]
> Ensures only 1 instance of the class exists and also provides a single point of access to it

This design pattern is used when the goal is to block the creation of new instances and keep a single point of creation. This point of creation will also have the logic to allow only 1 instance.

##### Singleton Pattern
First of all, we need to make the constructor of the singleton class private. This way the constructor is not accessible from outside the class and hence object creation is restricted to within the class

Now to actually create the 1 single object, we can define a method that calls this private constructor. Let's call this method as `getInstance()` and <mark style="background: #D2B3FFA6;">it is always supposed to be public and static (associated with the class and not the object)</mark>

The code inside will create a private static instance of the class (private because no access from outside and static because again associated with class level), and only that will be returned whenever any instance is requested

There are a couple of logics that you can find here - 

###### 1. Eager Initialization

	You already create an instance before-hand and then whenever getInstance() is called, you return that instance.

```Java
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

###### 2. Lazy Initialization (Non-thread safe)

	You create the instance only when requested. And once it is created, you return the same one

```Java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

Cons - if 2 or more threads access this method at the same time, you will have 2 or more objects

###### 3. Lazy Initialization (synchronized)

	Making the getInstance() method synchronized to avoid issues in multi-threading

```Java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

Cons - `synchronized` makes the process slow first by checking how many threads want to access it and then allowing only 1

###### 4. Double Checked Locking

	We avoid going in synchronized block by checking null outside the synchronized block but we also check it again inside synchronized block

```Java
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

> [!info]+ Instruction Reordering
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
> So ultimately <mark style="background: #FFB86CA6;">what volatile does is avoid the reordering which can lead to performance issue</mark>

###### 5. Bill Pugh Singleton (Inner static class)

	It provides lazy initialization and is thread-safe without requiring explicit synchronization. The core idea behind the Bill Pugh Singleton is to leverage the Java Virtual Machine's (JVM) guarantees about class initialization.

```Java
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

<mark style="background: #FFB8EBA6;">The `Holder` inner class is not loaded by the JVM until it is explicitly referenced.</mark> This means the INSTANCE of Bill Pugh Singleton is only created when `getInstance() `is called for the very first time. 

<mark style="background: #FFB8EBA6;">The `classloader` ensures that `Holder.INSTANCE` is initialized only once and only when needed.</mark>

> [!INFO]+  Why It's Thread-safe Without Synchronization
> Because of how the JVM loads classes:
> - Java guarantees that class initialization is thread-safe.
> - Static fields (like INSTANCE) are initialized once per classloader, in a thread-safe way.
> - Therefore, no need for synchronized or volatile.
> This relies on the Java Language Specification (JLS), which mandates safe publication of static class members.

###### Advantages
- **JVM Guarantees Class Initialization**: The JVM specification guarantees that a class will be initialized only once, and this initialization process is inherently thread-safe. When multiple threads try to access the `getInstance()` method simultaneously for the very first first time, they will all try to load the `SingletonHelper` class. However, the JVM ensures that only one thread will successfully initialize the class, and others will wait.
- **Lazy Loading of Inner Class**: The `SingletonHelper` class is loaded only when `getInstance()` is invoked. Until then, the `BillPughSingleton` instance is not created, thus providing lazy initialization.
- **No Explicit Synchronization**: Because the JVM handles the synchronization during class loading, you don't need to use `synchronized` keywords on the `getInstance()` method or `volatile` for the instance variable. This avoids the performance overhead often associated with explicit synchronization.
---
#### Builder Design Pattern

	Allows step by step construction/assignment of each field before finally returning the object


- It is used to construct complex objects step by step, separating the construction process from the final object representation.
- It allows you to build different versions of an object using the same building process.
- Here, we extract the object construction code out of its own class and use another object inside the constructor

```Java
public class Car {
    private final String model;
    private final boolean automatic;

    private Car(Builder builder) {
        this.model = builder.model;
        this.automatic = builder.automatic;
    }

    //Inner static Builder class
    public static class Builder {
        private String model;
        private boolean automatic;

        public Builder setModel(String model) {
            this.model = model;
            return this;
        }

        public Builder setAutomatic(boolean automatic) {
            this.automatic = automatic;
            return this;
        }

        public Car build() {
            return new Car(this);
        }
    }
}
```

---
#### Factory Method Design Pattern

	Separates the object creation code from the code that uses it, by centralizing object creation logic, especially when there are multiple possible classes to instantiate, and the decision of which class to create depends on input, configuration, or runtime conditions.

> Spring use this pattern heavily behind the scenes (for beans).

Now let's stick to our `Vehicle` interface and `Car` and `Bus` class implementing it and a method somewhere which needs to create either `Car` or `Bus` object based on some input

So ideally you would do this - 
```Java
public Vehicle createVehicle(String type) {
    if (type.equals("car")) {
        return new Car();
    } else if (type.equals("bus")) {
        return new Bus();
    }
    return null;
}
```

<mark style="background: #D2B3FFA6;">You need to modify code everywhere if a new vehicle type is added or a vehicle type is removed. Clear violation of Open-Closed Principle</mark>

In Factory Design Pattern, you centralize this logic of creating objects in 1 place - 

```Java
public class VehicleFactory {
    public static Vehicle getVehicle(String type) {
        if (type.equalsIgnoreCase("car")) {
            return new Car();
        } else if (type.equalsIgnoreCase("bus")) {
            return new Bus();
        }
        throw new IllegalArgumentException("Unknown vehicle type");
    }
}

//and now you can simply call - 
Vehicle vehicle = VehicleFactory.createVehicle(type);
```

<mark style="background: #ABF7F7A6;">This is called as Simple Factory (not Factory Method Design Pattern).</mark>
But wait it stills violates the OCP because if new vehicle is added or removed then we will have to modify it

##### Factory Method Design Pattern
To implement it, we will create an interface first and then have Car and Bus Factory classes implement it - 
```Java
interface VehicleFactory{
	Vehicle createVehicle();
}

public class CarFactory implements VehicleFactory {
    public Vehicle createVehicle() {
        return new Car();
    }
}

public class BusFactory implements VehicleFactory {
    public Vehicle createVehicle() {
        return new Bus();
    }
}
```

This way you won't violate OCP. You won't need to modify anything from above code if a new `Train` vehicle is added, you will create its `TrainFactory` class by implementing `VehicleFactory` 

```Java
public class Main {
    public static void main(String[] args) {
        VehicleFactory factory;

        // based on condition
        String input = "car";  // could be from user or config
        if (input.equalsIgnoreCase("car")) {
            factory = new CarFactory();
        } else {
            factory = new BusFactory();
        }

        Vehicle vehicle = factory.createVehicle();
        vehicle.start();
    }
}
```

##### Factory Registry 
Just like Prototype Registry, you can create this - 

```Java
Map<String, VehicleFactory> registry = new HashMap<>();
registry.put("car", new CarFactory());
registry.put("bus", new BusFactory());

VehicleFactory factory = registry.get(input.toLowerCase());
if (factory == null) throw new IllegalArgumentException("Unknown vehicle type");

Vehicle vehicle = factory.createVehicle();
vehicle.start();
```

#### Abstract Factory Design Pattern

	Extension of Factory Design Pattern for creating families of objects

In our previous example for Factory Method Design Pattern we had Car and Bus classes. Now let's add attribute to it.
Say now we have 2 `Car` - `Electric` and `Diesel` 
and similarly, 2 `Bus` - `Electric` and `Diesel`

```Java
interface Car {
    void drive();
}

interface Bus {
    void drive();
}

class ElectricCar implements Car {
    public void drive() {
        System.out.println("Driving an electric car");
    }
}

class DieselCar implements Car {
    public void drive() {
        System.out.println("Driving a diesel car");
    }
}

class ElectricBus implements Bus {
    public void drive() {
        System.out.println("Driving an electric bus");
    }
}

class DieselBus implements Bus {
    public void drive() {
        System.out.println("Driving a diesel bus");
    }
}

interface VehicleFactory {
    Car createCar();
    Bus createBus();
}

class ElectricVehicleFactory implements VehicleFactory {
    public Car createCar() {
        return new ElectricCar();
    }
    public Bus createBus() {
        return new ElectricBus();
    }
}

class DieselVehicleFactory implements VehicleFactory {
    public Car createCar() {
        return new DieselCar();
    }
    public Bus createBus() {
        return new DieselBus();
    }
}
```

