#system-design-v2 #design-patterns #behavioral-design-pattern 

## Glossary
- Design Pattern where the main object (c/a *observable*/publisher) maintains a list of dependents (c/a *observers*/subscribers)
- The observable automatically notifies the observers when it's state changes
- <mark style="background: #FF6699;">It’s mainly used when multiple components need to react to the same event without being tightly coupled.</mark>
- This pattern allows the system to stay flexible - observable knows observers through a common interface. It doesn’t care how observers handle updates. 
- This makes observers independent and observers can be added or removed at runtime
##### How it works
**Observable**: The object whose state is being watched. It maintains a list of its observers and manages the registration and unregistration of observers.
**Observer**: The object that needs to be notified of the subject's state changes.
**Attachment/Detachment**: Observers can subscribe or unsubscribe from the subject, allowing the list of dependents to change dynamically.
**update()**: The key method that the observable calls to notify observers. Observers implement this method to react to the change.

***Examples***:
- Weather Applications
- Social Media
- YouTube Subscriptions
- **Kafka**

---
> [!note] 
> There are 2 models of implementations:
> - <span style="color:rgb(106, 240, 214)">Push</span> : Observable pushes the data to Observers when the state changes
> - <span style="color:rgb(106, 240, 214)">Pull</span> : Observers maintain a reference of Observable and pull when the state changes 

#### Push Model 
- Here the `observable` maintains a list of `observers` 
- Whenever the data changes, the `observable` calls the `update(data)` for all `observers`

![[Push Observer Design Pattern for UML Class Diagram.png|450]]

#### Pull Model (less preferrable imo)
- Here the `observer` maintains a reference of  `observable`
- The `observable` still maintains a list of `observers`
- When data is changed, we iterate over the `observers` list and call the `observer.update()` 
- However the difference here is, the `update()` doesn't need the data as argument, it can retrieve the data using the `observable` reference


![[Pull Observer Design Pattern UML Class Diagram.png|450]]