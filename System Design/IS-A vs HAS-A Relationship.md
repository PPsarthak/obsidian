#system-design-v2 

>**Not diving much into IS-A Relationship here....**

Exalidraw contains some notes too... [[_Excalidraw/IS-A vs HAS-A Relationship|IS-A vs HAS-A Relationship]]
### Difference
- Both relationship define how classes relate to each other
- An "is-a" relationship signifies inheritance, where one class (subclass or child class) extends or implements another class or interface (superclass or parent class/interface). *It signifies inheritance*
- A "has-a" relationship signifies association, where one class contains an instance of another class as a member variable. It represents that an object is composed of or uses other objects. *It signifies association*


![[IS-A vs HAS-A Example.png]] 

## HAS-A Relationship
### Association
- Association is the relation between two separate classes which establishes through their Objects. 
- **Composition** and **Aggregation** are the two forms of association. 
- It can be unidirection or bidirectional

> [!note] 
> Association can be considered a generic term to indicate the relationship between two independent classes; **the relationship may be one-to-one, one-to-many, or many-to-many, but it need not indicate ownership**. 

#### Aggregation
- Aggregation is a specific form of association in which one class (called as "whole") contains other class / classes (called as "parts")
- However, the *lifecycle of the parts does not depend upon the whole.* Hence, it is a form of association with **weaker relationship strength**
- For example, a library and books show aggregation, since books may exist somewhere apart from the library.
- It is a **unidirectional association**

#### Composition
- Composition is a stronger form of aggregation that means ownership and lifecycle dependence
- If the whole gets destroyed, then the parts no longer exist.  Hence, it is a form of association with **stronger relationship strength**
- For example, a house and rooms show composition, a room cannot exist without a house.

![[Aggregation vs Composition.png]]