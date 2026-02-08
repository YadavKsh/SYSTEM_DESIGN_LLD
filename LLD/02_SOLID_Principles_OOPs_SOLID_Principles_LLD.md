# 🎯 SOLID Principles

---

## 📚 SOLID

- **S** - Single Responsibility Principle
- **O** - Open / Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segmented Principle
- **D** - Dependency Inversion Principle

### ✨ Advantages of following these principles:

1. Help us to write **better code**
2. Avoid **duplicate code**
3. Easy to **maintain**
4. Easy to **understand**
5. **Flexible** software
6. Reduce **complexity**

---

## 1️⃣ Single Responsibility Principle (SRP)

> **A class should have only one reason to change.**

### 🏷️ Marker Entity

```java
class Marker {
    String name;
    String color;
    int year;
    int price;
    
    public Marker(String name, String color, int year, int price) {
        this.name = name;
        this.color = color;
        this.year = year;
        this.price = price;
    }
}
```

### ❌ Invoice class (Violating SRP)

```java
class Invoice {
    private Marker marker;
    private int quantity;
    
    public Invoice(Marker marker, int quantity) {
        this.marker = marker;
        this.quantity = quantity;
    }
    
    public int calculateTotal() {
        int price = ((marker.price) * this.quantity);
        return price;
    }
    
    public void printInvoice() {
        // print the Invoice
    }
    
    public void saveToDB() {
        // Save the data into DB
    }
}
```

> **⚠️ Problem:** The above Invoice class has **3 reasons to change**, so it doesn't follow Single Responsibility Principle.

### ✅ Solution: How can it follow SRP?

#### (i) Invoice class (only calculation)

```java
class Invoice {
    private Marker marker;
    private int quantity;
    
    public Invoice(Marker marker, int quantity) {
        this.marker = marker;
        this.quantity = quantity;
    }
    
    public int calculateTotal() {
        int price = ((marker.price) * this.quantity);
        return price;
    }
}
```

#### (ii) InvoiceDao class (only database operations)

```java
class InvoiceDao {
    Invoice invoice;
    
    public InvoiceDao(Invoice invoice) {
        this.invoice = invoice;
    }
    
    public void saveToDB() {
        // Save the data into DB
    }
}
```

#### (iii) InvoicePrinter class (only printing)

```java
class InvoicePrinter {
    private Invoice invoice;
    
    public InvoicePrinter(Invoice invoice) {
        this.invoice = invoice;
    }
    
    public void printInvoice() {
        // print the Invoice
    }
}
```

---

## 2️⃣ Open/Closed Principle (OCP)

> **Open for Extension but Closed for Modification**

### ❌ InvoiceDao class (Violating OCP)

```java
class InvoiceDao {
    Invoice invoice;
    
    public InvoiceDao(Invoice invoice) {
        this.invoice = invoice;
    }
    
    public void saveToDB() {
        // Save the data into DB
    }
}
```

### ❌ Modified InvoiceDao (Still Violating OCP)

```java
class InvoiceDao {
    Invoice invoice;
    
    public InvoiceDao(Invoice invoice) {
        this.invoice = invoice;
    }
    
    public void saveToDB() {
        // Save the data into DB
    }
    
    public void saveToFile(String file_name) {
        // Save invoice in the file with the given name
    }
}
```

> **⚠️ Problem:** This is not following Open/Closed Principle as we are **modifying** the pre-written code (which might be tested and live in production) instead of **extending** it.

### ✅ Solution: How can it follow OCP?

#### Create an interface

```java
interface InvoiceDao {
    public void save(Invoice invoice);
}
```

#### Implement for Database

```java
class DatabaseInvoiceDao implements InvoiceDao {
    @Override
    public void save(Invoice invoice) {
        // Save to DB
    }
}
```

#### Implement for File

```java
class FileInvoiceDao implements InvoiceDao {
    @Override
    public void save(Invoice invoice) {
        // Save to file
    }
}
```

---

## 3️⃣ Liskov Substitution Principle (LSP)

> **Key Points:**
> - If class B is a subtype of class A, then we should be able to **replace object of A with B** without breaking the behaviour of the program.
> - Subclass should **extend** the capability of parent class, not **narrow** it down.

### ❌ Example (Violating LSP)

```java
interface Bike {
    void turnOnEngine();
    void accelerate();
}
```

```java
class Motorcycle implements Bike {
    boolean isEngineOn;
    int speed;
    
    public void turnOnEngine() {
        // turn on the engine!
        isEngineOn = true;
    }
    
    public void accelerate() {
        // increase the speed
        speed = speed + 10;
    }
}
```

```java
class Bicycle implements Bike {
    
    public void turnOnEngine() {
        throw new AssertionError("there is no engine");
    }
    
    public void accelerate() {
        // do something
    }
}
```

> **⚠️ Problem:** It breaks the behaviour of the code because if someone calls `bicycle.turnOnEngine()`, the user expects the engine to be turned on but instead it's **throwing an error**.

---

## 4️⃣ Interface Segmented Principle (ISP)

> **Interfaces should be such that clients should not implement unnecessary functions they do not need.**

### ❌ RestaurantEmployee interface (Violating ISP)

```java
interface RestaurantEmployee {
    void washDishes();
    void serveCustomers();
    void cookFood();
}
```

```java
class Waiter implements RestaurantEmployee {
    
    public void washDishes() {
        // not my job
    }
    
    public void serveCustomers() {
        // yes and here is my implementation
        System.out.println("serving the customer");
    }
    
    public void cookFood() {
        // not my job
    }
}
```

> **⚠️ Problem:** This breaks the implementation because it is **not the waiter's job** to wash dishes or cook food. But since it is implementing the interface, the client (waiter) has to implement all those functions.

### ✅ Solution: How can it follow ISP?

#### Create segregated interfaces

```java
interface WaiterInterface {
    void serveCustomers();
    void takeOrder();
}

interface ChefInterface {
    void cookFood();
    void decideMenu();
}
```

#### Waiter Implementation

```java
class Waiter implements WaiterInterface {
    
    public void serveCustomers() {
        System.out.println("serving the customers");
    }
    
    public void takeOrder() {
        System.out.println("taking orders");
    }
}
```

#### Chef Implementation

```java
class Chef implements ChefInterface {
    
    public void cookFood() {
        System.out.println("cooking food");
    }
    
    public void decideMenu() {
        System.out.println("deciding menu");
    }
}
```

---

## 5️⃣ Dependency Inversion Principle (DIP)

> **Class should depend on interfaces rather than concrete classes.**

### 🔧 Interface Hierarchy

```
INTERFACES
    ↓
Mouse Interface → Wired Mouse, Bluetooth Mouse
Keyboard Interface → Wired Keyboard, Bluetooth Keyboard
```

### ❌ Macbook class (Violating DIP)

```java
class Macbook {
    private final WiredKeyboard keyboard;
    private final WiredMouse mouse;
    
    public Macbook() {
        keyboard = new WiredKeyboard();
        mouse = new WiredMouse();
    }
}
```

> **⚠️ Problem:** Since keyboard is of type `WiredKeyboard`, in the future even if we want to change it to `BluetoothKeyboard`, we **can't** because it's tightly coupled to `WiredKeyboard`.

### ✅ Solution: How can it follow DIP?

```java
class Macbook {
    private final Keyboard keyboard;
    private final Mouse mouse;
    
    public Macbook(Keyboard keyboard, Mouse mouse) {
        this.keyboard = keyboard;
        this.mouse = mouse;
    }
}
```

> **💡 Benefit:** Now we can pass **any type** of keyboard (Wired or Bluetooth) and mouse, making the code flexible and following Dependency Inversion Principle!

---

## 📝 Summary

| Principle | Key Concept |
|-----------|-------------|
| **SRP** | One class, one responsibility |
| **OCP** | Open for extension, closed for modification |
| **LSP** | Subtypes must be substitutable for their base types |
| **ISP** | Many specific interfaces are better than one general interface |
| **DIP** | Depend on abstractions, not concretions |

---

**✨ Remember:** Following SOLID principles leads to **cleaner**, **more maintainable**, and **flexible** code!