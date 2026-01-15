#system-design-v2 

#### Creational Design Pattern
- Controls how and when objects are created
- Type:
	- Singleton
	- Builder
	- Factory
	- Abstract Factory
	- Object Pool
	- Prototype

#### Structural Design Pattern
- Controls how objects are arranged/structured in a large system
- Type:
	- Decorator
	- Proxy
	- Composite
	- Adapter
	- Bridge
	- Facade
	- Flyweight

#### Behavioral Design Pattern
- Controls how objects behave or interact with each other
- Type:
	- State
	- Strategy
	- Observer
	- Chain of Responsibility
	- Template
	- Iterator
	- Interpreter
	- Command
	- Visitor
	- Mediator
	- Memento
	- Null Object

### When to use which patterns:
🧩 General OOP / Concurrency Patterns

| **Pattern**           | **When to Use**                                                                            |
| --------------------- | ------------------------------------------------------------------------------------------ |
| *Interface*             | When multiple classes share common methods but each has a different implementation.        |
| *Abstract Parent Class* | When some common behavior is shared but subclasses need to implement specific methods.     |
| *Private Methods*       | To break complex logic into smaller helper methods not accessible outside the class.       |
| *Accessor Methods*      | To provide controlled access (get/set) to object data (encapsulation).                     |
| *Constant Data Manager* | To store constants in a class for global access without instantiation.                     |
| *Immutable Object*      | When object state should not change after creation; ensures consistency and thread-safety. |
| *Monitor*               | When multiple threads access shared data; synchronized methods ensure safe access.         |

🧩  Creational Patterns

| **Pattern**      | **When to Use**                                                                    |
| ---------------- | ---------------------------------------------------------------------------------- |
| *Singleton*        | When only one instance of a class is needed globally.                              |
| *Factory Method*   | When a class wants to delegate object creation to subclasses.                      |
| *Abstract Factory* | When creating families of related objects without specifying concrete classes.     |
| *Prototype*        | When new similar objects are created efficiently by cloning.                       |
| *Builder*          | When constructing complex objects step-by-step, possibly with optional parameters. |

🧩  Collectional / Structural Patterns

| **Pattern** | **When to Use**                                                       |
| ----------- | --------------------------------------------------------------------- |
| *Composite*   | When treating individual and composite objects uniformly.             |
| *Flyweight*   | When reducing memory usage by sharing common objects.                 |
| *Visitor*     | When adding operations to classes without modifying their code.       |
| *Iterator*    | When traversing a collection without exposing its internal structure. |

🧩 Structural Patterns

| **Pattern**             | **When to Use**                                                            |
| ----------------------- | -------------------------------------------------------------------------- |
| *Decorator*               | When dynamically adding functionality without modifying the class.         |
| *Adapter*                 | When making incompatible interfaces work together.                         |
| *Facade*                  | When exposing a simple interface to a complex system.                      |
| *Object Release*          | When releasing expensive resources after use.                              |
| *Object Cache*            | When reusing already created objects to boost performance.                 |
| *Proxy*                   | When delaying object creation or controlling access to it.                 |
| *Bridge*                  | When separating abstraction from implementation for independent evolution. |
| *Chain of Responsibility* | When multiple handlers can process a request in sequence.                  |
| *Aggregator Enforcer*     | When all modifications must pass through a single root object.             |

🧩Behavioral Patterns

| **Pattern**               | **When to Use**                                                          |
| ------------------------- | ------------------------------------------------------------------------ |
| *Command*                   | When requests should be treated as objects (supports queue, log, undo).  |
| *Mediator*                  | When centralizing communication among highly interactive objects.        |
| *State*                     | When behavior depends on the current internal state.                     |
| *Memento*                   | When capturing and restoring an object’s past state.                     |
| *Observer*                  | When objects must react automatically to another object’s state changes. |
| *Interpreter*               | When processing grammar-based sentences or expressions.                  |
| *Strategy*                  | When swapping algorithms or behaviors interchangeably.                   |
| *Null Object*               | When avoiding null checks by providing a default no-op object.           |
| *Common Attribute Registry* | When a centralized configuration or shared data store is needed.         |
| *Template*                  | When defining an algorithm structure with customizable steps.            |
| *Object Authenticator*      | When controlling or validating access to an object or its data.          |
