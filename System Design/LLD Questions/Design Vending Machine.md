#system-design-v2 #design-questions 

### 1. Problem Scope
Design a Vending Machine that sells items. The machine behaves differently based on its current state:
- When no money is inserted, the user cannot select an item.
- After money is inserted, the user can select an item.
- If an item is dispensed, the machine returns to the idle state.
- If the item is out of stock, the behavior changes again.

### 2. Functional Requirements
**Product Management**: 
- The vending machine should support multiple product types (for example: snacks, drinks, chocolates). Each product must have:
	- A unique product code
	- Name
	- Price
- The system should maintain inventory count for each product.
- A product cannot be dispensed if it is out of stock.

**Product Selection**:
- Users should be able to select a product using its product code.
- The system must validate:
	- Product exists
	- Product is in stock
	- Sufficient balance has been inserted
	- If balance is insufficient, the system should prompt for additional money.

**Money Handling**:
- Users should be able to insert money incrementally.
- The system should track the total inserted amount for the current transaction.

**Dispensing**:
- Upon successful validation:
	- The selected product should be dispensed
	- Inventory should be updated

**Change Management**:
- If the inserted amount exceeds the product price:
	- The machine should return correct change
- If exact change is not possible:
	- The transaction should fail gracefully
	- Inserted money should be refunded

**Transaction Control**:
- The user should be able to cancel the transaction at any time before dispensing.
- On cancellation:
	- The full inserted amount should be returned
	- No inventory should be updated
### 3. Entities and Responsibilites

| Entities        | Responsibilites                                 |
| --------------- | ----------------------------------------------- |
| Product         | Contains attributes - product code, name, price |
| Vending Machine | Handles inventory logic and orchestration       |
| Vending State   | Handles payment, dispensing, updating inventory |

### 4. Design Patterns
- State

### 5. UML Class Diagram
![[UML Class Diagram for Vending Machine.png|1250]]

### 6. Code
```java
public abstract class Product {
    private final String code;
    private final String name;
    private final double price;

    protected Product(String code, String name, double price) {
        this.code = code;
        this.name = name;
        this.price = price;
    }
}

public class Chocolate extends Product {
    public Chocolate(String code, String name, double price) {
        super(code, name, price);
    }
}

public class Chips extends Product {
    public Chips(String code, String name, double price) {
        super(code, name, price);
    }
}

public class Drink extends Product {
    public Drink(String code, String name, double price) {
        super(code, name, price);
    }
}

public class VendingMachine {

    private VendingState state;
    private final Map<Product, Integer> inventory = new HashMap<>();
    private double balance;
    private Product selectedProduct;

    public VendingMachine() {
        this.state = new InventoryState();
    }

    public void addProduct(Product product, int quantity) {
        inventory.put(product, quantity);
    }

    public void insertMoney(double amount) {
        balance += amount;
    }

    public void selectProduct(Product product) {
        state.handle(this, product);
    }

    void setState(VendingState state) {
        this.state = state;
    }

    void setSelectedProduct(Product product) {
        this.selectedProduct = product;
    }

    void reset() {
        balance = 0;
        selectedProduct = null;
        state = new InventoryState();
    }

    // getters for states
    Map<Product, Integer> getInventory() { return inventory; }
    double getBalance() { return balance; }
}

public interface VendingState {
    void handle(VendingMachine machine, Product product);
}

public class InventoryState implements VendingState {

    @Override
    public void handle(VendingMachine machine, Product product) {
        Integer qty = machine.getInventory().get(product);

        if (qty == null || qty <= 0) {
            throw new RuntimeException("Product out of stock");
        }

        machine.setSelectedProduct(product);
        machine.setState(new PaymentState());
    }
}

public class PaymentState implements VendingState {

    @Override
    public void handle(VendingMachine machine, Product product) {
        double price = product.getPrice();

        if (machine.getBalance() < price) {
            throw new RuntimeException("Insufficient balance");
        }

        machine.setState(new DispenserState());
    }
}


public class DispenserState implements VendingState {

    @Override
    public void handle(VendingMachine machine, Product product) {
        // dispense
        System.out.println("Dispensing: " + product.getName());

        // update inventory
        machine.getInventory().put(
                product,
                machine.getInventory().get(product) - 1
        );

        // reset machine
        machine.reset();
    }
}

```

### 7. Interview Questions

##### 1️⃣ Why did you choose the State design pattern here?

**Answer:**
The vending machine’s behavior changes based on its internal state (Inventory → Payment → Dispenser). Using the State pattern removes conditional logic (`if/else`) from the main class and encapsulates state-specific behavior, making the system easier to extend and maintain.

---

##### 2️⃣ Why does `VendingState` receive `VendingMachine` as a parameter?

**Answer:**
States need access to the context to:

* Change the current state
* Read/update inventory
* Reset the machine
  This keeps transitions **inside states**, which is the core idea of the State pattern.

---

##### 3️⃣ Why not return `boolean` from `handle()`?

**Answer:**
Returning booleans hides *why* an operation failed. State transitions are side effects, not results. Errors are better handled via exceptions or explicit result objects for clarity and extensibility.

---

##### 4️⃣ Where does inventory belong and why?

**Answer:**
Inventory belongs to `VendingMachine` (context), not states.
States **use** inventory but don’t **own** it. This avoids duplication and keeps shared data centralized.

---

##### 5️⃣ How would you add a “Cancel / Refund” feature?

**Answer:**
Add a `RefundState` that:

* Returns inserted money
* Clears selected product
* Transitions back to `InventoryState`
  No existing states need modification.

---

##### 6️⃣ How would you support multiple payment types (cash, card, UPI)?

**Answer:**
Use **Strategy pattern** inside `PaymentState`, where each payment method implements a `PaymentStrategy`.
State handles flow; strategy handles payment logic.

---

##### 7️⃣ What happens if a product goes out of stock during payment?

**Answer:**
Revalidate inventory in `PaymentState` before dispensing. If unavailable, transition to `RefundState` and notify the user.

---

##### 8️⃣ Is this design thread-safe?

**Answer:**
No, not by default. To make it thread-safe:

* Synchronize state transitions
* Use locks around inventory updates
* Or ensure one request per machine instance

---

##### 9️⃣ How is State different from Strategy here?

**Answer:**

* **State** changes behavior based on internal machine state
* **Strategy** swaps algorithms (e.g., payment methods)
  State controls flow; Strategy controls logic.

---

##### 🔟 How would you persist inventory and transactions?

**Answer:**
Use a repository layer (DB or cache) outside the machine. The vending machine should remain a pure domain model.

---

##### 🔚 Bonus: Biggest design trade-off?

**Answer:**
More classes and indirection, but in exchange:

* Clean separation of concerns
* Easier extensibility
* Interview-friendly, scalable design

---
