#system-design-v2 

> Reference - Medium Blogs by user - https://medium.com/@danaprata 
> Each Principle is present as separate article -  [S](https://medium.com/@danaprata/the-s-in-solid-principles-c11b443c4bcd), [O](https://medium.com/@danaprata/the-o-in-solid-principles-73fb6d730be0), [L](https://medium.com/@danaprata/the-l-in-solid-principles-6d55cf101cdb), [I](https://medium.com/@danaprata/the-i-in-solid-principles-aee7f5034a8a), [D](https://medium.com/@danaprata/the-d-in-solid-principles-d98d0a91cd2d)

### S - Single Responsibility Principle (SRP)
Every software component (module, class, method) must have a single responsibility 
And hence, it can have ONE AND ONLY ONE reason to change ^44a090

Example,
```Java
public class User{
  public void register(){...}

  public void passwordForgotten(){...}

  public void sendEmail(){...}
}
```

Here the User class has 3 responsibilities - register user, password forget and send email
If in the future, you need to change the logic of password forgotten, you will need to change the User class, this is a violation of the SRP Principle

Here is the solution,
```Java
public class UserRegistration{
  public void register(){...}
}

public class UserPasswordForgotten{
  public void passwordForgotten(){...}
}

public class UserEmailSending{
  public void sendEmail(){...}
}
```

This way, each class has 1 sole responsibility and it also has ONE AND ONLY ONE reason to change - change in business logic of that class

### O - Open/Closed Principle (OCP)
The OCP states that software components should be OPEN for EXTENSION but CLOSED for MODIFICATION. In oder words, we should be able to ADD new FEATURES/behaviour WITHOUT BREAKING the existing code.

Here is an example,
```Java
public class Shape {
    private String type;

    public Shape(String type) {
        this.type = type;
    }

    public void draw() {
        if (type.equals("circle")) {
            drawCircle();
        } else if (type.equals("rectangle")) {
            drawRectangle();
        } else {
            System.out.println("Unsupported shape: " + type);
        }
    }

    private void drawCircle() {
        System.out.println("Drawing a circle");
    }

    private void drawRectangle() {
        System.out.println("Drawing a rectangle");
    }
}

public class Main {
    public static void main(String[] args) {
        Shape circle = new Shape("circle");
        circle.draw();

        Shape rectangle = new Shape("rectangle");
        rectangle.draw();

        Shape triangle = new Shape("triangle");
        triangle.draw();
    }
}
```

Shape class violates the OCP because when a new shape type (e.g., triangle) is added, you need to modify the existing Shape class by adding a new condition in the draw() method.

Solution, create an interface `Shape` with `draw()` abstract method and then create each individual class for each shape that implements the draw()
```Java
public abstract class Shape {  
	public abstract void draw();  
}  
  
public class Circle extends Shape {  
	@Override  
	public void draw() {  
		System.out.println("Drawing a circle");  
	}  
}  
  
public class Rectangle extends Shape {  
	@Override  
	public void draw() {  
		System.out.println("Drawing a rectangle");  
	}  
}  
  
public class Triangle extends Shape {  
	@Override  
	public void draw() {  
		System.out.println("Drawing a triangle");  
	}  
}
```
### L - Liskov Substitution Principle (LSP)
 LSP states that a SUBCLASS should be able to REPLACE its SUPERCLASS WITHOUT causing any PROBLEMS or unexpected behavior.
Meaning the subclass should not change the logic of super class

Example,
```Java
// LSP Violation Example
class Bird {
    public void fly() {
        System.out.println("I can fly.");
    }
}

class Ostrich extends Bird {
    // Ostrich cannot fly, but inherits a fly() method that implies it can.
    // This violates LSP because substituting an Ostrich for a Bird might lead to unexpected behavior.
    
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostriches cannot fly.");
    }
}

public static void main(String[] args) {
	Bird bird1 = new Bird();
	Bird bird2 = new Ostrich(); // Substituting Ostrich for Bird

	try {
		bird1.fly(); // Works as expected
		bird2.fly(); // Throws UnsupportedOperationException, violating LSP
	} catch (UnsupportedOperationException e) {
		System.out.println("Error: " + e.getMessage());
	}
}
```

Here is the corrected code
```Java
interface Flyable {
    void fly();
}

class Bird {
    // General bird behaviors, not including flying if not all birds fly
    public void eat() {
        System.out.println("I can eat.");
    }
}

class FlyingBird extends Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("I can fly.");
    }
}

class Ostrich extends Bird {
    // Ostriches are birds but do not implement Flyable
}

public static void main(String[] args) {
	Bird bird1 = new FlyingBird();
	Bird bird2 = new Ostrich();

	// If we need to make birds fly, we only operate on Flyable instances
	if (bird1 instanceof Flyable) {
		((Flyable) bird1).fly(); // Works for FlyingBird
	}

	if (bird2 instanceof Flyable) {
		// This block will not be executed for Ostrich, preventing unexpected behavior
		((Flyable) bird2).fly();
	} else {
		System.out.println("Ostrich cannot fly.");
	}
}
```

### I - Interface Segregation Principle (ISP)
The ISP states that CLIENTS should NOT be forced to DEPEND on INTERFACES they DO NOT USE.

Here is an example,
```Java
interface Machine {
    void start();
    void stop();
    void print();
    void scan();
}
```

Here we have an interface Machine and let's say we are having classes that implementing Machine to create CPU, Printer, Scanner, Speakers, etc.
Clearly, a CPU and Speakers don't need print() and scan()
So this violates ISP because these 2 classes will still have to implement these 2 methods

Solution - create separate interfaces for each functionality

```java
interface StartMachine {
	void start();
}

interface StopMachine {
	void stop();
}

interface PrintMachine {
	void print();
}

interface ScanMachine {
	void scan();
}
```

### D - Dependency Inversion Principle (DIP)
DIP states that HIGH-LEVEL MODULES should NOT DEPEND on LOW-LEVEL MODULES. Both should DEPEND on ABSTRACTIONS.
In other words, the DIP suggests that the direction of dependency between modules should be inverted. Instead of high-level modules depending on low-level modules, both should depend on abstractions or interfaces.

Example,
```Java
// High-level class
class WeatherStation {
    private WeatherService weatherService;

    public WeatherStation() {
        this.weatherService = new WeatherService();
    }

    public void getTemperature() {
        int temperature = weatherService.fetchTemperature();
        System.out.println("Current temperature: " + temperature + " degrees Celsius.");
    }
}

// Low-level class
class WeatherService {
    public int fetchTemperature() {
        // Code to fetch temperature from a specific weather API
        return 25;
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherStation weatherStation = new WeatherStation();
        weatherStation.getTemperature();
    }
}
```

Here Weather Station is tightly coupled with Weather Service class and hence High Level class depends on Low level class

Solution, 
```Java
// Abstraction
interface WeatherDataProvider {
    int fetchTemperature();
}

// Hig-level class
class WeatherStation {
    private WeatherDataProvider weatherDataProvider;

    public WeatherStation(WeatherDataProvider weatherDataProvider) {
        this.weatherDataProvider = weatherDataProvider;
    }

    public void getTemperature() {
        int temperature = weatherDataProvider.fetchTemperature();
        System.out.println("Current temperature: " + temperature + " degrees Celsius.");
    }
}

// Low-level class
class WeatherService implements WeatherDataProvider {
    public int fetchTemperature() {
        // Code to fetch temperature from a specific weather API
        return 25;
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherDataProvider weatherDataProvider = new WeatherService();
        WeatherStation weatherStation = new WeatherStation(weatherDataProvider);
        weatherStation.getTemperature();
    }
}
```

In this modified version, we’ve introduced the WeatherDataProvider interface, which serves as an abstraction. The WeatherService class now implements this interface, and the WeatherStation class depends on the abstraction (WeatherDataProvider) instead of the concrete implementation (WeatherService).

The WeatherStation class receives an instance of WeatherDataProvider through its constructor, which allows us to easily switch between different implementations of WeatherDataProvider .

> [!summary] 
> Another possible example of this might be the JPARepository Interface which acts as an abstraction between a service-layer and the actual database
> A service could directly communicate with database and fetch entities directly but JPA acts as an intermediate agent (+ it also provides sophisticated methods) 