#system-design-v2 #design-questions 

### 1. Problem Scope
Design a Pizza Billing System for a pizza store that allows customers to:
- Choose a pizza (type/size)
- Add or remove toppings / select extras (cheese burst, stuffed crust, etc.)
- Generate a final bill with accurate pricing

### 2. Functional Requirements
1. Pizza Creation
	- User can select a base pizza
2. Customization
	- Add one or more toppings
	- Add extras (e.g., extra cheese)
	- Each customization affects the final price
3. Price Calculation
	- System calculates total price dynamically
	- Each component contributes independently

### 3. Entities and Responsibilites
| Entity / Class       | Type                       | Responsibilities                                                                                  | Design Notes                                                                      |
| -------------------- | -------------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| **Pizza**            | Interface (Component)      | - Defines contract for all pizzas and decorators<br>- Exposes `getCost()` and `getDescription()`  | Core abstraction. Enables polymorphism and decorator chaining.                    |
| **Toppings**         | Abstract Class (Decorator) | - Implements `Pizza`<br>- Wraps another `Pizza` instance<br>- Acts as base class for all toppings | Represents **Decorator** in Decorator Pattern. Ensures is-a + has-a relationship. |
| **CheeseBurstPizza** | Concrete Class             | - Represents a base pizza variant<br>- Provides base cost and description                         |                                                                                   |
| **ThinCrustPizza**   | Concrete Class             | - Represents a base pizza variant<br>- Provides base cost and description                         |                                                                                   |
| **MargheritaPizza**  | Concrete Class             | - Represents a base pizza variant<br>- Provides base cost and description                         |                                                                                   |
| **MushroomToppings** | Concrete Decorator         | - Adds mushroom topping cost<br>- Appends topping description                                     |                                                                                   |
| **VeggieToppings**   | Concrete Decorator         | - Adds veggie topping cost<br>- Appends topping description                                       |                                                                                   |
| **PaneerToppings**   | Concrete Decorator         | - Adds paneer topping cost<br>- Appends topping description                                       |                                                                                   |
| **Biller**           | Service / Utility Class    | - Generates final bill using `Pizza` abstraction<br>- Prints order details                        |                                                                                   |
| **Client**           | Client / Driver            | - Creates pizzas and applies toppings<br>- Demonstrates decorator chaining                        |                                                                                   |


### 4. Design Patterns
- Decorator

### 5. UML Class Diagram
![[UML Class Diagram for Pizza Billing System.png]]
### 6. Code
```java
public interface Pizza {
    double getCost();
    String getDescription();
}

public abstract class Toppings implements Pizza {
    protected Pizza pizza;

    public Toppings(Pizza pizza) {
        this.pizza = pizza;
    }
}

public class CheeseBurstPizza implements Pizza{
    public static final double PRICE = 180;
    public static final String DESCRIPTION = "Cheese Burst Pizza";

    @Override
    public double getCost() {
        return PRICE;
    }

    @Override
    public String getDescription() {
        return DESCRIPTION;
    }
}

public class ThinCrustPizza implements Pizza{
    public static final double PRICE = 150;
    public static final String DESCRIPTION = "Thin Crust Pizza";

        @Override
    public double getCost() {
        return PRICE;
    }

    @Override
    public String getDescription() {
        return DESCRIPTION;
    }
}


public class MargheritaPizza implements Pizza{
    public static final double PRICE = 120;
    public static final String DESCRIPTION = "Margherita Pizza";

	@Override
    public double getCost() {
        return PRICE;
    }

    @Override
    public String getDescription() {
        return DESCRIPTION;
    }
}

public class MushroomToppings extends Toppings{
    public static final double PRICE = 40;
    public static final String DESCRIPTION = " + Mushroom Toppings";

    public MushroomToppings(Pizza pizza) {
        this.pizza = pizza;
    }

    @Override
    public double getCost() {
        return pizza.getCost() + getPRICE();
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + " " + getDESCRIPTION();
    }
}

public class VeggieToppings extends Toppings{
    public static final double PRICE = 35;
    public static final String DESCRIPTION = " + Veggie Toppings";

    public VeggieToppings(Pizza pizza) {
        this.pizza = pizza;
    }

    @Override
    public double getCost() {
        return pizza.getCost() + PRICE;
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + " " + DESCRIPTION;
    }
}

public class PaneerToppings extends Toppings{
    public static final double PRICE = 55;
    public static final String DESCRIPTION = " + Paneer Toppings";

    public PaneerToppings(Pizza pizza) {
        this.pizza = pizza;
    }

    @Override
    public double getCost() {
        return pizza.getCost() + PRICE;
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + " " + DESCRIPTION;
    }
}

public class Biller {
    public double generateBill(Pizza pizza) {
        System.out.println("You purchased: " + pizza.getDescription());
        return pizza.getCost();
    }
}

public class Client {
    public static void main(String[] args) {
        Biller biller = new Biller();

        Pizza pizza1 = new CheeseBurstPizza();
        System.out.println(biller.generateBill(pizza1));

        Pizza pizza2 = new PaneerToppings(new MargheritaPizza());
        System.out.println(biller.generateBill(pizza2));

        Pizza pizza3 = new VeggieToppings(new PaneerToppings(new MargheritaPizza()));
        System.out.println(biller.generateBill(pizza3));
    }
}
```


### 7. Interview Questions



