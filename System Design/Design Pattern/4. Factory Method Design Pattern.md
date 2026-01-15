#system-design-v2 #design-patterns  #creational-design-pattern

## Glossary
- Used when we want to place all the logic and flow of object creation in one place
- There are 2 approaches of Factory Design Pattern
	- Simple Factory Pattern
	- Factory Method Pattern
### Simple Factory Pattern
- Here we define a common interface `Shape` and concrete implementations of it
- A `ShapeFactory` class handles object creation logic and returns the required object
#### UML Class Diagram
![[UML Class Diagram for Simple Factory Design Pattern.png|650]]

##### Problems
- The `ShapeFactory` code <mark style="background: #33EE99;">violates Open-Closed Principle</mark>; because when a new shape `Triangle` needs to be added we will have to modify the existing code
- It also <mark style="background: #33EE99;">violates Single Responsibility Principle</mark>; becuase if the logic for object initialization is complex (i.e., it includes validating the request, configuring somethings before creating object); then the code has 2 responsibilities - selection of the shape + construction of shape

### Factory Method Pattern
- Here we define common interface `Factory` and 2 concrete implementations of it for each shape
- In the `ShapeFactoryMethod` we decide which shape needs to be created first; and then call the respective `Factory` implementations to create the `Shape` object
- Here, `ShapeFactoryMethod` has the single responsibilty of selection of shape and the `ShapeFactory` has the single responsibility of construction of shape

#### UML Class Diagram
![[UML Class Diagram for Factory Method Design Pattern.png|700]]

##### Problem
- This still <mark style="background: #33EE99;">violates Open Closed Principle</mark>; because when a new shape `Triangle` needs to be added we will have to modify the existing code

## Abstract Factory Design Pattern
-  This pattern is commonly used when we start using the Factory Method Pattern, and we need to evolve our system to a more complex system. 
- *It supports customization of object creation* which is missing in Factory Method Pattern