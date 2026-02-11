# 🏭 Factory Pattern vs Abstract Factory Pattern

A comprehensive guide to understanding and implementing these essential creational design patterns.

---

## 🔧 Factory Pattern

### What is it?

The **Factory Pattern** is a creational design pattern that provides an interface for creating objects without specifying their exact classes. It delegates the instantiation logic to a factory class.

### 🎯 Intent

- Encapsulate object creation logic
- Decouple client code from concrete classes
- Centralize object creation in one place

### 📊 Structure Diagram

```
┌─────────────────┐
│     Client      │
└────────┬────────┘
         │
         │ uses
         ▼
┌─────────────────┐
│  ProductFactory │
└────────┬────────┘
         │
         │ creates
         ▼
┌─────────────────┐
│    Product      │ (interface)
└────────┬────────┘
         │
    ┌────┴────┬────────────┐
    ▼         ▼            ▼
┌────────┐ ┌────────┐ ┌────────┐
│Product │ │Product │ │Product │
│   A    │ │   B    │ │   C    │
└────────┘ └────────┘ └────────┘
```

### 💻 Code Example

```java
// Product Interface
interface Vehicle {
    void drive();
}

// Concrete Products
class Car implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a car 🚗");
    }
}

class Bike implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Riding a bike 🚴");
    }
}

class Truck implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a truck 🚚");
    }
}

// Factory Class
class VehicleFactory {
    public static Vehicle createVehicle(String type) {
        switch (type.toLowerCase()) {
            case "car":
                return new Car();
            case "bike":
                return new Bike();
            case "truck":
                return new Truck();
            default:
                throw new IllegalArgumentException("Unknown vehicle type");
        }
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        Vehicle vehicle1 = VehicleFactory.createVehicle("car");
        vehicle1.drive(); // Output: Driving a car 🚗
        
        Vehicle vehicle2 = VehicleFactory.createVehicle("bike");
        vehicle2.drive(); // Output: Riding a bike 🚴
    }
}
```

### ✅ Advantages

- **Single Responsibility**: Object creation logic is separated from business logic
- **Easy to extend**: Adding new products requires minimal changes
- **Code reusability**: Centralized creation logic
- **Loose coupling**: Client depends on interfaces, not concrete classes

### ❌ Disadvantages

- Can become complex with many product types
- May introduce unnecessary abstraction for simple cases

---

## 🏗️ Abstract Factory Pattern

### What is it?

The **Abstract Factory Pattern** is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

### 🎯 Intent

- Create families of related objects
- Ensure products from the same family are used together
- Support multiple product families (themes, platforms, etc.)

### 📊 Structure Diagram

```
┌─────────────────┐
│     Client      │
└────────┬────────┘
         │
         │ uses
         ▼
┌──────────────────┐
│ AbstractFactory  │ (interface)
└────────┬─────────┘
         │
    ┌────┴────┬──────────────┐
    ▼         ▼              ▼
┌─────────┐ ┌─────────┐ ┌─────────┐
│Factory1 │ │Factory2 │ │Factory3 │
└────┬────┘ └────┬────┘ └────┬────┘
     │           │            │
     │creates    │creates     │creates
     │           │            │
┌────▼─────┐ ┌──▼──────┐ ┌──▼──────┐
│ProductA1 │ │ProductA2│ │ProductA3│
└──────────┘ └─────────┘ └─────────┘
┌──────────┐ ┌─────────┐ ┌─────────┐
│ProductB1 │ │ProductB2│ │ProductB3│
└──────────┘ └─────────┘ └─────────┘
```

### 💻 Code Example

```java
// Abstract Products
interface Button {
    void render();
}

interface Checkbox {
    void render();
}

// Concrete Products - Windows Family
class WindowsButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering Windows button 🪟");
    }
}

class WindowsCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering Windows checkbox ☑️");
    }
}

// Concrete Products - Mac Family
class MacButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering Mac button 🍎");
    }
}

class MacCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering Mac checkbox ✅");
    }
}

// Abstract Factory
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete Factories
class WindowsFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

class MacFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}

// Client Code
public class Application {
    private Button button;
    private Checkbox checkbox;
    
    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }
    
    public void render() {
        button.render();
        checkbox.render();
    }
    
    public static void main(String[] args) {
        String os = "Windows"; // or "Mac"
        
        GUIFactory factory;
        if (os.equals("Windows")) {
            factory = new WindowsFactory();
        } else {
            factory = new MacFactory();
        }
        
        Application app = new Application(factory);
        app.render();
        // Output:
        // Rendering Windows button 🪟
        // Rendering Windows checkbox ☑️
    }
}
```

### ✅ Advantages

- **Consistency**: Ensures related products are used together
- **Isolation**: Separates concrete classes from client code
- **Easy to swap families**: Change entire product family at once
- **Single Responsibility**: Each factory handles one family

### ❌ Disadvantages

- Increased complexity with many interfaces and classes
- Adding new products requires changing all factories

---

## 🔍 Key Differences

| Aspect | Factory Pattern | Abstract Factory Pattern |
|--------|----------------|-------------------------|
| **Purpose** | Create single objects | Create families of related objects |
| **Complexity** | Simple, one level | More complex, multiple levels |
| **Products** | One product hierarchy | Multiple product hierarchies |
| **Method** | Single factory method | Multiple factory methods |
| **Focus** | "What to create" | "How to create related objects" |
| **Use Case** | Creating one type of product | Creating multiple related products |
| **Example** | Vehicle Factory (Car, Bike, Truck) | GUI Factory (Button + Checkbox for Windows/Mac) |

### 📊 Visual Comparison

```
Factory Pattern:
┌─────────────┐
│   Factory   │──► Creates ──► Product A
└─────────────┘           ├──► Product B
                          └──► Product C

Abstract Factory Pattern:
┌─────────────────┐
│ Abstract Factory│
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌────────┐
│Factory1│ │Factory2│
└───┬────┘ └───┬────┘
    │          │
    ├─► A1     ├─► A2
    └─► B1     └─► B2
```

---

## 🎯 When to Use Each Pattern

### Use Factory Pattern When:

✅ You have a single product hierarchy
✅ Object creation logic is complex or may change
✅ You want to decouple object creation from usage
✅ You need centralized object creation
✅ Product types are determined at runtime

**Example Scenarios:**
- Creating different types of documents (PDF, Word, Excel)
- Database connection factories (MySQL, PostgreSQL, MongoDB)
- Logger factories (File, Console, Network)

### Use Abstract Factory Pattern When:

✅ You need to create families of related objects
✅ Products must be used together (consistency required)
✅ You want to support multiple platforms or themes
✅ You need to ensure compatibility between products
✅ System should be independent of product creation

**Example Scenarios:**
- Cross-platform UI components (Windows, Mac, Linux)
- Theme systems (Light theme, Dark theme with multiple components)
- Database access layers with multiple vendors
- Game environments (Medieval, SciFi, Fantasy with consistent items)

---

## 🌍 Real-World Examples

### Factory Pattern Example: Payment Processing

```java
interface PaymentMethod {
    void processPayment(double amount);
}

class CreditCard implements PaymentMethod {
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via Credit Card 💳");
    }
}

class PayPal implements PaymentMethod {
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via PayPal 🅿️");
    }
}

class Cryptocurrency implements PaymentMethod {
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via Crypto ₿");
    }
}

class PaymentFactory {
    public static PaymentMethod getPaymentMethod(String type) {
        switch (type.toLowerCase()) {
            case "card": return new CreditCard();
            case "paypal": return new PayPal();
            case "crypto": return new Cryptocurrency();
            default: throw new IllegalArgumentException("Invalid payment type");
        }
    }
}
```

### Abstract Factory Example: Furniture Shop

```java
// Abstract Products
interface Chair { void sitOn(); }
interface Sofa { void lieOn(); }
interface Table { void placeItems(); }

// Modern Family
class ModernChair implements Chair {
    public void sitOn() { System.out.println("Sitting on modern chair 🪑"); }
}
class ModernSofa implements Sofa {
    public void lieOn() { System.out.println("Lying on modern sofa 🛋️"); }
}
class ModernTable implements Table {
    public void placeItems() { System.out.println("Placing items on modern table ⬜"); }
}

// Victorian Family
class VictorianChair implements Chair {
    public void sitOn() { System.out.println("Sitting on Victorian chair 👑"); }
}
class VictorianSofa implements Sofa {
    public void lieOn() { System.out.println("Lying on Victorian sofa 🎭"); }
}
class VictorianTable implements Table {
    public void placeItems() { System.out.println("Placing items on Victorian table 🏛️"); }
}

// Abstract Factory
interface FurnitureFactory {
    Chair createChair();
    Sofa createSofa();
    Table createTable();
}

// Concrete Factories
class ModernFurnitureFactory implements FurnitureFactory {
    public Chair createChair() { return new ModernChair(); }
    public Sofa createSofa() { return new ModernSofa(); }
    public Table createTable() { return new ModernTable(); }
}

class VictorianFurnitureFactory implements FurnitureFactory {
    public Chair createChair() { return new VictorianChair(); }
    public Sofa createSofa() { return new VictorianSofa(); }
    public Table createTable() { return new VictorianTable(); }
}
```

---

## 🎓 Quick Decision Guide

```
Do you need to create a single object?
    │
    ├─ YES ──► Use Factory Pattern
    │
    └─ NO
        │
        Do you need to create multiple related objects?
            │
            ├─ YES ──► Use Abstract Factory Pattern
            │
            └─ NO ──► Consider other patterns
```

---

## 📚 Summary

### Factory Pattern
- **One sentence**: A pattern to create objects without specifying exact class
- **Key benefit**: Centralized object creation
- **When**: Single product type with variations

### Abstract Factory Pattern
- **One sentence**: A pattern to create families of related objects
- **Key benefit**: Ensures compatibility between related products
- **When**: Multiple product types that must work together

---

## 💡 Best Practices

1. **Factory Pattern**
    - Keep factory methods simple
    - Use enums or constants for product types
    - Consider returning interfaces, not concrete classes
    - Document which products are available

2. **Abstract Factory Pattern**
    - Design product families carefully
    - Ensure all concrete factories implement all methods
    - Use dependency injection for factories
    - Plan for extensibility from the start

---

## 🚀 Conclusion

Both patterns are powerful tools for managing object creation. Choose based on your specific needs:

- **Simple object creation** → Factory Pattern
- **Related object families** → Abstract Factory Pattern

Remember: Start simple with Factory Pattern, and evolve to Abstract Factory when you need to manage product families! 🎯

---

*Happy Coding! 👨‍💻👩‍💻*