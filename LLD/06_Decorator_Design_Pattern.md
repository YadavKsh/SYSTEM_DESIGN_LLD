# 🎨 Decorator Pattern

## 🤔 Problem Statement

### ⚠️ The Challenge

Imagine you're building a coffee shop ordering system. You have a basic `Coffee` class, but customers want to add various extras:
- ☕ Milk
- 🍦 Whipped cream
- 🍯 Caramel
- 🍫 Chocolate
- 💪 Extra espresso shot

**The Naive Approach:**
You might think to create a class for every combination:
- `CoffeeWithMilk`
- `CoffeeWithMilkAndCaramel`
- `CoffeeWithMilkAndWhippedCream`
- `CoffeeWithMilkAndCaramelAndWhippedCream`
- ... and so on

This quickly becomes a **class explosion problem** 💥. With just 5 add-ons, you'd need potentially 2^5 = 32 different classes!

### 🎯 Real-World Problems

The Decorator pattern solves several key issues:

1. **🔧 Extending functionality without modifying existing code** - You need to add responsibilities to objects dynamically without altering their class
2. **🚫 Avoiding class explosion** - Creating subclasses for every combination becomes unmanageable
3. **🎯 Flexible composition** - You want to mix and match features at runtime
4. **🔒 Maintaining the Open/Closed Principle** - Classes should be open for extension but closed for modification

## 💡 What is the Decorator Pattern?

The Decorator pattern is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects (decorators) that contain the behaviors.

### ✨ Key Characteristics

- 📦 **Wraps** an object to add new functionality
- 🔄 **Maintains** the same interface as the wrapped object
- 🏗️ **Allows** stacking multiple decorators
- 🎭 **Preserves** the object's type from the client's perspective

## 🏛️ Structure

```
┌─────────────────────┐
│   <<interface>>     │
│      Coffee         │
├─────────────────────┤
│ + cost(): double    │
│ + description(): String │
└──────────┬──────────┘
           │
           │ implements
           │
    ┌──────┴────────┐
    │               │
┌───▼────────┐  ┌───▼──────────────┐
│SimpleCoffee│  │CoffeeDecorator   │
├────────────┤  │   (abstract)     │
│+ cost()    │  ├──────────────────┤
│+ desc()    │  │- coffee: Coffee  │
└────────────┘  │+ cost()          │
                │+ desc()          │
                └────────┬─────────┘
                         │ extends
                         │
           ┌─────────────┼─────────────┐
           │             │             │
    ┌──────▼──────┐ ┌───▼─────┐ ┌────▼──────────┐
    │MilkDecorator│ │Caramel  │ │WhippedCream   │
    │             │ │Decorator│ │Decorator      │
    ├─────────────┤ ├─────────┤ ├───────────────┤
    │+ cost()     │ │+ cost() │ │+ cost()       │
    │+ desc()     │ │+ desc() │ │+ desc()       │
    └─────────────┘ └─────────┘ └───────────────┘
```

**Flow Diagram:**
```
SimpleCoffee ($5)
    ↓ wrapped by
MilkDecorator ($5 + $1 = $6)
    ↓ wrapped by
CaramelDecorator ($6 + $1.5 = $7.5)
    ↓ wrapped by
WhippedCreamDecorator ($7.5 + $0.75 = $8.25)
    ↓
Final Coffee: "Simple coffee, milk, caramel, whipped cream" - $8.25
```

### 🧩 Components:

1. **Component** 📋 - Defines the interface for objects that can have responsibilities added
2. **ConcreteComponent** 🎯 - The original object to which additional responsibilities can be attached
3. **Decorator** 🎁 - Maintains a reference to a Component object and defines an interface that conforms to Component's interface
4. **ConcreteDecorator** ⭐ - Adds responsibilities to the component

## 💻 Example Implementation

### ☕ Coffee Shop Example (Java)

```java
// Component Interface
public interface Coffee {
    double cost();
    String description();
}

// Concrete Component
public class SimpleCoffee implements Coffee {
    @Override
    public double cost() {
        return 5.0;
    }
    
    @Override
    public String description() {
        return "Simple coffee";
    }
}

// Base Decorator (Abstract)
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
    
    @Override
    public double cost() {
        return coffee.cost();
    }
    
    @Override
    public String description() {
        return coffee.description();
    }
}

// Concrete Decorators
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public double cost() {
        return coffee.cost() + 1.0;
    }
    
    @Override
    public String description() {
        return coffee.description() + ", milk";
    }
}

public class CaramelDecorator extends CoffeeDecorator {
    public CaramelDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public double cost() {
        return coffee.cost() + 1.5;
    }
    
    @Override
    public String description() {
        return coffee.description() + ", caramel";
    }
}

public class WhippedCreamDecorator extends CoffeeDecorator {
    public WhippedCreamDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public double cost() {
        return coffee.cost() + 0.75;
    }
    
    @Override
    public String description() {
        return coffee.description() + ", whipped cream";
    }
}

// Usage Example
public class CoffeeShop {
    public static void main(String[] args) {
        // Start with simple coffee
        Coffee myCoffee = new SimpleCoffee();
        System.out.println(myCoffee.description()); // "Simple coffee"
        System.out.println("Cost: $" + myCoffee.cost()); // Cost: $5.0
        
        // Add milk
        myCoffee = new MilkDecorator(myCoffee);
        
        // Add caramel
        myCoffee = new CaramelDecorator(myCoffee);
        
        // Add whipped cream
        myCoffee = new WhippedCreamDecorator(myCoffee);
        
        System.out.println(myCoffee.description()); 
        // "Simple coffee, milk, caramel, whipped cream"
        System.out.println("Cost: $" + myCoffee.cost()); // Cost: $8.25
    }
}
```

## 🌍 Real-World Use Cases

### 1️⃣ Java I/O Streams
```java
BufferedReader reader = new BufferedReader(
    new FileReader(
        new File("data.txt")
    )
);
```

### 2️⃣ UI Components
- 📜 Adding scrollbars to windows
- 🖼️ Adding borders to panels
- 🌑 Adding shadows to elements

### 3️⃣ Text Formatting
- **📝 Adding bold, italic, underline to text**
- ✨ Applying multiple formatting styles

### 4️⃣ Logging and Monitoring
- ⏰ Adding timestamp to logs
- 📍 Adding context information
- 🔐 Adding encryption

## ✅ Advantages

1. **🎯 Flexibility** - More flexible than static inheritance
2. **📌 Single Responsibility** - Each decorator has a single purpose
3. **🧩 Composability** - You can combine decorators in different ways
4. **⚡ Runtime Configuration** - Behavior can be added/removed at runtime
5. **🔓 Open/Closed Principle** - Extend functionality without modifying existing code

## ❌ Disadvantages

1. **🤯 Complexity** - Many small objects that can be hard to understand
2. **🐛 Debugging** - Stack of decorators can be difficult to debug
3. **🔢 Order Dependency** - Sometimes the order of decorators matters
4. **🎭 Identity** - Decorated object is not identical to the original

## 🎯 When to Use

Use the Decorator pattern when:

✔️ You need to add responsibilities to objects dynamically and transparently  
✔️ Extension by subclassing is impractical or would lead to class explosion  
✔️ You want to add or remove functionality at runtime  
✔️ You want to avoid a large hierarchy of classes with small differences

## 🚫 When NOT to Use

Avoid the Decorator pattern when:

❌ You have a simple system with few variations  
❌ You need to change the core functionality (not just extend it)  
❌ The order of decorators creates too much complexity  
❌ Performance is critical (decorators add overhead)

## ⚖️ Comparison with Similar Patterns

### 🎨 Decorator vs Adapter
- **Decorator** ➕ Adds new functionality while keeping the same interface
- **Adapter** 🔌 Makes incompatible interfaces work together

### 🎨 Decorator vs Proxy
- **Decorator** ✨ Adds functionality
- **Proxy** 🚪 Controls access to the object

### 🎨 Decorator vs Composite
- **Decorator** 🎁 Adds responsibilities (usually just one object)
- **Composite** 🌳 Treats groups of objects uniformly

---

## 👀 Bonus: Observer Design Pattern Diagram

Since you asked about the Observer pattern, here's a visual comparison:

### 🔄 Key Differences: Decorator vs Observer

| Aspect | 🎨 Decorator | 👀 Observer |
|--------|-------------|------------|
| **Purpose** | Add responsibilities to objects | Notify multiple objects of state changes |
| **Relationship** | Wrapper wraps component | Subject notifies observers |
| **Direction** | Single object, layered wrapping | One-to-many notification |
| **Use Case** | Enhance individual object behavior | Event handling, pub-sub systems |
| **Example** | Adding features to a coffee order | Stock price updates to multiple displays |

---

## 📝 Summary

The Decorator pattern is your go-to solution when you need flexible, runtime-configurable object enhancement without the nightmare of creating numerous subclasses for every possible combination of features. It's elegant, follows SOLID principles, and is widely used in many frameworks and libraries. 🚀✨