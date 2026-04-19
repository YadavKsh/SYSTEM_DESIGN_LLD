# 🏗️ SOLID Principles — Notes

Software development is a complex field that requires careful planning and design to ensure applications are **maintainable**, **scalable**, and **easy to understand**. SOLID principles are five essential guidelines that enhance software design.

---

## 📋 The 5 SOLID Principles

| # | Principle | Short Meaning |
|---|-----------|---------------|
| 1 | 🔑 **Single Responsibility Principle (SRP)** | One class = one job |
| 2 | 🔒 **Open/Closed Principle (OCP)** | Open for extension, closed for modification |
| 3 | 🔄 **Liskov Substitution Principle (LSP)** | Subtypes must be replaceable for their base types |
| 4 | 🔧 **Interface Segregation Principle (ISP)** | Don't force classes to implement unused methods |
| 5 | 🔗 **Dependency Inversion Principle (DIP)** | Depend on abstractions, not concretions |

---

## 💡 Why SOLID Matters in OOP

| Benefit | Description |
|---------|-------------|
| **Scalability** 📈 | Adding new features becomes straightforward |
| **Maintainability** 🔄 | Changes in one part have minimal impact on others |
| **Testability** 🧪 | Decoupled designs make unit testing easier |
| **Readability** 📖 | Clear separation of concerns improves code comprehension |

---

## 1️⃣ Single Responsibility Principle (SRP)

> **"A class should have only one reason to change."**
> Every class should have a **single responsibility**, a single job, or a single purpose within the software system.

### 🌟 Why it Matters
- Improves maintainability — changes in one class do not affect unrelated parts
- Enhances readability and reduces complexity

---

### ❌ Bad Example — Violating SRP

The `BreadBaker` class does too many things: baking, managing inventory, ordering supplies, serving customers, and cleaning. Any change to one responsibility risks breaking the others.

```java
// Class with multiple responsibilities — VIOLATES SRP
class BreadBaker {
    public void bakeBread()        { System.out.println("Baking high-quality bread..."); }
    public void manageInventory()  { System.out.println("Managing inventory..."); }
    public void orderSupplies()    { System.out.println("Ordering supplies..."); }
    public void serveCustomer()    { System.out.println("Serving customers..."); }
    public void cleanBakery()      { System.out.println("Cleaning the bakery..."); }

    public static void main(String[] args) {
        BreadBaker baker = new BreadBaker();
        baker.bakeBread();
        baker.manageInventory();
        baker.orderSupplies();
        baker.serveCustomer();
        baker.cleanBakery();
    }
}
```

**Problem:** If inventory management changes, `BreadBaker` must be modified — even though baking bread is unrelated. This coupling increases the risk of bugs and makes code harder to maintain.

---

### ✅ Good Example — Following SRP

Each class now has exactly **one responsibility**:

```java
// Class for baking bread
class BreadBaker {
    public void bakeBread() { System.out.println("Baking high-quality bread..."); }
}

// Class for managing inventory
class InventoryManager {
    public void manageInventory() { System.out.println("Managing inventory..."); }
}

// Class for ordering supplies
class SupplyOrder {
    public void orderSupplies() { System.out.println("Ordering supplies..."); }
}

// Class for serving customers
class CustomerService {
    public void serveCustomer() { System.out.println("Serving customers..."); }
}

// Class for cleaning the bakery
class BakeryCleaner {
    public void cleanBakery() { System.out.println("Cleaning the bakery..."); }
}

public class Bakery {
    public static void main(String[] args) {
        BreadBaker baker                   = new BreadBaker();
        InventoryManager inventoryManager  = new InventoryManager();
        SupplyOrder supplyOrder            = new SupplyOrder();
        CustomerService customerService    = new CustomerService();
        BakeryCleaner cleaner              = new BakeryCleaner();

        // Each class focuses on its specific responsibility
        baker.bakeBread();
        inventoryManager.manageInventory();
        supplyOrder.orderSupplies();
        customerService.serveCustomer();
        cleaner.cleanBakery();
    }
}
```

**Result:** Each class has a clear and focused responsibility — the system is more **modular**, **readable**, and **easier to manage**.

| Class | Responsibility |
|-------|---------------|
| `BreadBaker` | Solely bakes bread |
| `InventoryManager` | Handles inventory |
| `SupplyOrder` | Manages supply ordering |
| `CustomerService` | Handles customer interactions |
| `BakeryCleaner` | Responsible for cleanliness |

---

## 2️⃣ Open/Closed Principle (OCP)

> **"Software entities (classes, modules, functions) should be open for extension, but closed for modification."**
> You should be able to extend a class's behavior **without modifying it**.

### 🌟 Why it Matters
- Prevents breaking existing code
- Encourages reusable components

---

### ❌ Bad Example — Violating OCP

Adding a new shape (e.g., Triangle) requires **modifying** the existing `calculateArea()` method — a violation.

```java
// Incorrect approach — VIOLATES OCP
class Shape {
    private String type;

    public double calculateArea() {
        if (type.equals("circle")) {
            // Circle area calculation
        } else if (type.equals("rectangle")) {
            // Rectangle area calculation
        }
        // Adding a triangle requires modifying this method ❌
    }
}
```

**Problem:** Every new shape forces a modification to `Shape`, increasing risk of bugs and making code less flexible. The `Shape` class is tightly coupled to specific shape types.

---

### ✅ Good Example — Following OCP

Use an **abstract class or interface** so new shapes can be added by **extending**, not modifying:

```java
// Better approach — follows Open/Closed Principle
abstract class Shape {
    abstract double calculateArea();
    // We can also use an interface instead of an abstract class
}

class Circle extends Shape {
    private double radius;

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;

    @Override
    public double calculateArea() {
        return width * height;
    }
}

// Adding a new shape WITHOUT modifying existing code ✅
class Triangle extends Shape {
    private double base;
    private double height;

    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}
```

**Explanation:**
- **`Shape` (abstract base class):** Defines a common interface with abstract `calculateArea()` for all shapes
- **`Circle`:** Implements `calculateArea()` for circles
- **`Rectangle`:** Implements `calculateArea()` for rectangles
- **`Triangle`:** Added **without touching any existing code** — just extend `Shape`

**Result:** Adding a new shape like `Triangle` does not require modifying existing code. This **prevents breaking existing functionality** and encourages **reusable components**.

---

## 3️⃣ Liskov Substitution Principle (LSP)

> **"Derived or child classes must be substitutable for their base or parent classes."**
> Any class that is the child of a parent class should be usable in place of its parent **without any unexpected behavior**.
> *(Introduced by Barbara Liskov in 1987)*

### 🌟 Why it Matters
- Ensures **reliability** when using polymorphism
- Avoids **unexpected behaviors** in subclass implementations

---

### ❌ Bad Example — Violating LSP

`Bicycle` cannot meaningfully implement `startEngine()` — throwing an exception breaks the substitutability contract.

```java
// Problematic approach that violates LSP
class Vehicle {
    public void startEngine() {
        // Engine starting logic
    }
}

class Car extends Vehicle {
    @Override
    public void startEngine() {
        // Car-specific engine starting logic
    }
}

class Bicycle extends Vehicle {
    @Override
    public void startEngine() {
        // Problem: Bicycles don't have engines!
        throw new UnsupportedOperationException("Bicycles don't have engines");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle car = new Car();
        Vehicle bicycle = new Bicycle();

        System.out.println("Car:");
        car.startEngine();          // Output: Car engine started.

        System.out.println("Bicycle:");
        try {
            bicycle.startEngine();  // Throws UnsupportedOperationException ❌
        } catch (UnsupportedOperationException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

**Problem:** `Bicycle` cannot be substituted for `Vehicle` without causing unexpected behavior (runtime exception). When a subclass cannot fulfill its parent's contract, polymorphism breaks down.

---

### ✅ Good Example — Following LSP

Restructure the hierarchy using two specialized abstract classes: `EngineVehicle` and `NonEngineVehicle`.

```java
// Better approach following LSP
abstract class Vehicle {
    // Common vehicle behaviors
    public void move() {
        // Movement logic
    }
}

abstract class EngineVehicle extends Vehicle {
    public void startEngine() {
        // Engine starting logic
    }
}

abstract class NonEngineVehicle extends Vehicle {
    // No engine-related methods
}

class Car extends EngineVehicle {
    @Override
    public void startEngine() {
        // Car-specific engine starting logic
    }
}

class Bicycle extends NonEngineVehicle {
    // Bicycle-specific methods
    // No need to implement engine-related methods ✅
}

public class Main {
    public static void main(String[] args) {
        // Using EngineVehicle
        EngineVehicle car = new Car();
        car.startEngine(); // Output: Car-specific engine starting logic
        car.move();        // Output: Movement logic

        // Using NonEngineVehicle
        NonEngineVehicle bicycle = new Bicycle();
        bicycle.move();    // Output: Movement logic ✅
    }
}
```

**Class Roles:**

| Class | Role |
|-------|------|
| `Vehicle` | Abstract base — common behaviors like `move()` |
| `EngineVehicle` | Extends Vehicle — adds `startEngine()` for motorized vehicles |
| `NonEngineVehicle` | Extends Vehicle — no engine methods for non-motorized vehicles |
| `Car` | Extends `EngineVehicle` — implements `startEngine()` |
| `Bicycle` | Extends `NonEngineVehicle` — no engine responsibility |

**This design ensures:**
1. Each subtype fully satisfies the **behavioral contract** of its parent type
2. Client code can interact with either vehicle type **without unexpected behavior**
3. The inheritance hierarchy accurately models the **real-world domain**

---

## 4️⃣ Interface Segregation Principle (ISP)

> **"Do not force any client to implement an interface which is irrelevant to them."**
> Prefer many **small, client-specific interfaces** over one large general interface. Each interface should have a specific responsibility.
> *(First principle in SOLID that applies to interfaces rather than classes — similar to SRP)*

### 🌟 Why it Matters
- Reduces **unnecessary dependencies**
- Simplifies implementation for **specific use cases**

---

### ❌ Bad Example — Violating ISP

`BasicPrinter` is forced to implement `scan()` and `fax()` which it cannot support — leading to runtime exceptions.

```java
// Problematic approach that violates ISP
interface Machine {
    void print();
    void scan();
    void fax();
}

class AllInOnePrinter implements Machine {
    @Override public void print() { /* Printing functionality */ }
    @Override public void scan()  { /* Scanning functionality */ }
    @Override public void fax()   { /* Fax functionality */ }
}

class BasicPrinter implements Machine {
    @Override
    public void print() { /* Printing functionality */ }

    @Override
    public void scan() {
        // Problem: Basic printer can't scan!
        throw new UnsupportedOperationException("Cannot scan"); // ❌
    }

    @Override
    public void fax() {
        // Problem: Basic printer can't fax!
        throw new UnsupportedOperationException("Cannot fax"); // ❌
    }
}
```

**Problem:** `BasicPrinter` is burdened with irrelevant methods, leading to unnecessary complexity and potential runtime errors. Any new method added to `Machine` forces all implementors to update — even those that don't need it.

---

### ✅ Good Example — Following ISP

Split the fat interface into smaller, focused interfaces:

```java
// Better approach following ISP
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

interface FaxMachine {
    void fax();
}

// BasicPrinter only implements what it needs ✅
class BasicPrinter implements Printer {
    @Override
    public void print() {
        // Printing functionality
    }
}

// AllInOnePrinter implements all three interfaces ✅
class AllInOnePrinter implements Printer, Scanner, FaxMachine {
    @Override public void print() { /* Printing functionality */ }
    @Override public void scan()  { /* Scanning functionality */ }
    @Override public void fax()   { /* Fax functionality */ }
}
```

**Interface Breakdown:**

| Interface | Method(s) |
|-----------|-----------|
| `Printer` | `print()` |
| `Scanner` | `scan()` |
| `FaxMachine` | `fax()` |

**This design ensures:**
- Classes implement **only the interfaces they need** — no irrelevant methods
- Prevents **dummy implementations** and potential runtime errors
- Makes the system **flexible and maintainable** — smaller, focused interfaces
- Follows **interface cohesion** — each interface has a specific responsibility

---

## 5️⃣ Dependency Inversion Principle (DIP)

> **"High-level modules should not depend on low-level modules. Both should depend on abstractions."**
> **"Abstractions should not depend on details. Details should depend on abstractions."**
>
> Classes should rely on **interfaces or abstract classes** rather than concrete implementations — enabling more flexible and decoupled code.

### 🌟 Why it Matters
- Promotes **decoupled architecture**
- Facilitates **testing and maintainability**

---

### ❌ Bad Example — Violating DIP

`OrderService` directly instantiates concrete classes — tightly coupled to specific implementations.

```java
// Problematic approach that violates DIP
class EmailNotifier {
    public void sendEmail(String message) {
        // Configure SMTP
        // Set up email templates
        // Send email implementation
    }
}

class OrderService {
    private EmailNotifier emailNotifier;
    private DatabaseLogger logger;
    private InventorySystem inventory;

    public OrderService() {
        // Direct dependencies on concrete implementations ❌
        this.emailNotifier = new EmailNotifier();
        this.logger        = new DatabaseLogger();
        this.inventory     = new InventorySystem();
    }

    public void placeOrder(Order order) {
        inventory.updateStock(order);
        emailNotifier.sendEmail("Order #" + order.getId() + " placed successfully");
        logger.logTransaction("Order placed: " + order.getId());
    }
}
```

**Problems created by this design:**
1. **Tight coupling** — `OrderService` is bound to specific implementations; hard to swap out
2. **Difficult unit testing** — can't mock dependencies; relies on concrete classes
3. **Inflexibility** — any change to notification or logging requires modifying `OrderService`
4. **Hard to maintain** — tight coupling makes the code harder to evolve
5. **No flexibility** — cannot easily support different notification strategies (e.g., SMS, push)

---

### ✅ Good Example — Following DIP

Introduce **abstractions** and use **dependency injection** to decouple `OrderService` from specific implementations.

```java
// Better approach following DIP

// Abstractions
interface NotificationService {
    void sendNotification(String message);
}

interface LoggingService {
    void logMessage(String message);
    void logError(String error);
}

interface InventoryService {
    void updateStock(Order order);
    boolean checkAvailability(Product product);
}

// Concrete implementations
class EmailNotifier implements NotificationService {
    @Override
    public void sendNotification(String message) {
        // Email specific implementation
    }
}

class SMSNotifier implements NotificationService {
    @Override
    public void sendNotification(String message) {
        // SMS specific implementation
    }
}

class PushNotifier implements NotificationService {
    @Override
    public void sendNotification(String message) {
        // Push notification specific implementation
    }
}

class DatabaseLogger implements LoggingService {
    @Override
    public void logMessage(String message) {
        // Database logging implementation
    }

    @Override
    public void logError(String error) {
        // Error logging implementation
    }
}

// High-level module depends on abstractions, not concretions ✅
class OrderService {
    private final NotificationService notificationService;
    private final LoggingService loggingService;
    private final InventoryService inventoryService;

    // Constructor injection of dependencies
    public OrderService(NotificationService notificationService,
                        LoggingService loggingService,
                        InventoryService inventoryService) {
        this.notificationService = notificationService;
        this.loggingService      = loggingService;
        this.inventoryService    = inventoryService;
    }

    public void placeOrder(Order order) {
        try {
            // Check inventory
            if (inventoryService.checkAvailability(order.getProduct())) {
                // Process order
                inventoryService.updateStock(order);
                // Send notification
                notificationService.sendNotification("Order #" + order.getId() + " placed successfully");
                // Log success
                loggingService.logMessage("Order processed successfully: " + order.getId());
            }
        } catch (Exception e) {
            loggingService.logError("Error processing order: " + order.getId() + " - " + e.getMessage());
            throw e;
        }
    }
}

// Usage with dependency injection
NotificationService emailNotifier = new EmailNotifier();
LoggingService logger             = new DatabaseLogger();
InventoryService inventory        = new WarehouseInventoryService();
OrderService orderService         = new OrderService(emailNotifier, logger, inventory);
```

**Interfaces introduced:**

| Interface | Methods |
|-----------|---------|
| `NotificationService` | `sendNotification(String message)` |
| `LoggingService` | `logMessage(String message)`, `logError(String error)` |
| `InventoryService` | `updateStock(Order order)`, `checkAvailability(Product product)` |

**Benefits of this design:**

| Benefit | Description |
|---------|-------------|
| **Flexibility** | Easily swap notification methods (Email → SMS → Push) without touching `OrderService` |
| **Testability** | Dependencies can be **mocked** for unit testing |
| **Maintainability** | Each component has a **single responsibility** and can be modified independently |
| **Scalability** | New implementations can be added **without affecting existing code** |
| **Config Flexibility** | Different implementations can be **injected at runtime** based on conditions |

---

## 📊 SOLID Quick Reference Summary

| Principle | Key Rule | Analogy |
|-----------|----------|---------|
| **SRP** 🔑 | One class = one job | Baker only bakes |
| **OCP** 🔒 | Extend, don't modify | Add new shapes without changing old code |
| **LSP** 🔄 | Subtypes must honour parent contract | Bicycle shouldn't fake having an engine |
| **ISP** 🔧 | No fat interfaces | Printer shouldn't implement `fax()` |
| **DIP** 🔗 | Depend on abstractions | `OrderService` uses `NotificationService`, not `EmailNotifier` |

---