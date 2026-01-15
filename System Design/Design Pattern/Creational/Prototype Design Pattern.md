#system-design-v2 #design-patterns  #creational-design-pattern

## Glossary
- Creates new objects by copying an existing object (which is c/a **Prototype**)
- Used when new objects should always be a copy of existing object (new objects should follow a template)

##### Naive Approach
Say we have to duplicate an object of class `Car`(let's call it `carA`)
So ideally what we do is use the `new`keyword and create another object (call it `carB`) and set the fields using the same value as in `carA`. 

*Problem* : 
Firsty, this is the part of solution but we won't write this in client code (main class), becuase it may result in duplication of code. 
Also, if `Car` has 100 fields, who would set them manually.
Also, accessing `private` fields wouldn't be possible from outside of class (w/o getters)

#### Prototype Pattern
- The idea is to create an interface (c/a Prototype) which has 1 method - `clone()`
- This interface sits at the top of hierarchy chain of objects that will follow this pattern
- The concrete class implements the `clone()` and includes the logic to automatically create clone of the same object.
- The `clone()` will use a copy constructor

```java
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


> [!note] 
>  Say the Car has a field - `private List<String> features`
>  Now there are 2 ways to implement the `clone()`: shallow copy & deep copy (based on your preference)
>  ```java
>  @Override
>  public Car clone(){                 //SHALLOW COPY
> 	 return new Car(this.features);
>  }
//OR
> @Override
> public Car clone(){                 //DEEP COPY
>	return new Car(new ArrayList<>(this.features));
> }
>  ```

#### UML Class Diagram
![[UML Class Diagram for Prototype Design Pattern.png|450]]

##### Prototype Registry
> [!quote] 
> Centralized storage (usually a map/dictionary) where you store pre-configured instances, and then clone them on demand 

Say you have multiple `Car` objects - `carA`, `carB`, `carC` with little modifications in some fields. Think of this registry as hashmap that stores these perfect objects and then in client code you can choose either of the perfect `Car` object(s) available in the registry
