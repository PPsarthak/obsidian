#system-design-v2 #design-patterns  #creational-design-pattern

## Glossary
- Allows step by step construction/assignment of each field before finally returning the object
- It is used to construct complex objects step by step, separating the construction process from the final object representation.
- It allows you to build different versions of an object using the same building process.

> [!note]
> Spring use this pattern heavily behind the scenes (for beans)

```java
public class Car {
    private final String model;
    private final boolean automatic;

	private Car(Builder builder) {   //constructor contains Builder arg
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
