# 🏗️ SOLID Principles

## 📖 What are SOLID Principles?

**SOLID** is an acronym for **5 object-oriented design principles** introduced by **Robert C. Martin (Uncle Bob)**. These principles act as guidelines to write code that is **clean, maintainable, scalable, and easy to understand**. 🎯

They help developers avoid **code smells**, reduce **tight coupling**, and make systems **easier to extend** without breaking existing functionality.

---

## 🔤 What Does SOLID Stand For?

```
  S  →  Single Responsibility Principle  (SRP)
  O  →  Open / Closed Principle          (OCP)
  L  →  Liskov Substitution Principle    (LSP)
  I  →  Interface Segregation Principle  (ISP)
  D  →  Dependency Inversion Principle   (DIP)
```

---

## 💡 Why Do We Need SOLID Principles?

Without SOLID principles, code tends to become:

- 🔴 **Rigid** — hard to change without breaking other parts.
- 🔴 **Fragile** — one change breaks something unexpected elsewhere.
- 🔴 **Immobile** — hard to reuse components in other projects.
- 🔴 **Viscous** — easier to hack than to do the right thing.

With SOLID principles, code becomes:

- ✅ **Easy to maintain** — changes are isolated and predictable.
- ✅ **Easy to extend** — new features don't break existing code.
- ✅ **Easy to test** — components are loosely coupled and independent.
- ✅ **Easy to understand** — each class/module has a clear purpose.

---

## 🗂️ Quick Overview

| Principle | Full Name | One Line Summary |
|---|---|---|
| **S** | Single Responsibility | A class should have **only one reason to change**. |
| **O** | Open / Closed | Open for **extension**, closed for **modification**. |
| **L** | Liskov Substitution | Subclasses should be **replaceable** for their parent class. |
| **I** | Interface Segregation | Don't force a class to implement interfaces it **doesn't need**. |
| **D** | Dependency Inversion | Depend on **abstractions**, not on concrete implementations. |

---

## 🗂️ Key Takeaway

> 💡 SOLID principles are not strict rules — they are **guidelines** that lead to better software design. Applying them thoughtfully results in code that is **robust, flexible, and built to last**. 🏗️

---

## 1️⃣ S — Single Responsibility Principle (SRP)

- ⭐ A class should have **only one reason to change**.
- ⭐ A class should **do only one thing**.

---

### ❌ Bad Example — Violating SRP

`ShoppingCart` is doing **three things** at once — calculating price, printing invoice, AND saving to DB. That's three reasons to change. 🔴

```java
class ShoppingCart {
    List<Product> products;

    double calcTotalPrice() { }   // Responsibility 1 — Calculation
    void printInvoice() { }       // Responsibility 2 — Printing
    void saveToDB() { }           // Responsibility 3 — Storage
}
```

---

### ✅ Good Example — Following SRP

Split into **three separate classes**, each doing **only one thing**: 🟢

```
  +─────────────────────────+        +──────────────────────────────+
  │        Product          │        │        ShoppingCart          │
  +─────────────────────────+        +──────────────────────────────+
  │ - price: double         │◄──1..*─│ - products: List<Product>    │
  │ - name: String          │        +──────────────────────────────+
  +─────────────────────────+        │ + calcTotalPrice(): double   │
                                     +──────────────────────────────+
                                               ▲              ▲
                              uses             │              │    uses
                  +───────────────────────+    │    +─────────────────────+
                  │   CartInvoicePrinter  │────┘    │    CartDBStorage    │
                  +───────────────────────+         +─────────────────────+
                  │ - sc: ShoppingCart    │         │ - sc: ShoppingCart  │
                  │ + printInvoice()      │         │ + saveToDB()        │
                  +───────────────────────+         +─────────────────────+
```

---

### ☕ Java Code

```java
// Product class
class Product {
    double price;
    String name;

    Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
}

// Responsibility 1 — Only calculates total price
class ShoppingCart {
    List<Product> products = new ArrayList<>();

    void addProduct(Product p) {
        products.add(p);
    }

    double calcTotalPrice() {
        double total = 0;
        for (Product p : products) total += p.price;
        return total;
    }
}

// Responsibility 2 — Only prints the invoice
class CartInvoicePrinter {
    ShoppingCart sc;

    CartInvoicePrinter(ShoppingCart sc) {
        this.sc = sc;
    }

    void printInvoice() {
        System.out.println("Invoice:");
        for (Product p : sc.products) {
            System.out.println(p.name + " - " + p.price);
        }
        System.out.println("Total: " + sc.calcTotalPrice());
    }
}

// Responsibility 3 — Only saves to DB
class CartDBStorage {
    ShoppingCart sc;

    CartDBStorage(ShoppingCart sc) {
        this.sc = sc;
    }

    void saveToDB() {
        System.out.println("Saving cart to DB with total: " + sc.calcTotalPrice());
        // DB logic here
    }
}

// Main
public class SRPDemo {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        cart.addProduct(new Product("Apple", 1.5));
        cart.addProduct(new Product("Banana", 0.5));

        CartInvoicePrinter printer = new CartInvoicePrinter(cart);
        printer.printInvoice();  // Only printing ✅

        CartDBStorage storage = new CartDBStorage(cart);
        storage.saveToDB();      // Only saving ✅
    }
}
```

> 💡 Now each class has **exactly one reason to change** — if invoice format changes, only `CartInvoicePrinter` changes. If DB logic changes, only `CartDBStorage` changes. `ShoppingCart` stays untouched. 🎯

---

## 2️⃣ O — Open-Closed Principle (OCP)

- ⭐ A class should be **open for extension** but **closed for modification**.

---

### ❌ Bad Example — Violating OCP

Every time we need a new storage type, we **modify** `DBStorage` by adding more methods. This breaks existing tested code. 🔴

```java
class DBStorage {
    ShoppingCart sc;
 
    void saveToDB() { }       // existing
    void saveToMongo() { }    // added later — modifying the class ❌
    void saveToFile() { }     // added later — modifying the class ❌
}
```
 
---

### ✅ Good Example — Following OCP

Create an **abstract base** and **extend** it for each storage type. Never touch the original. 🟢

```
  +─────────────────────────+        +──────────────────────────────+
  │        Product          │        │            Cart              │
  +─────────────────────────+        +──────────────────────────────+
  │ - name: String          │◄──List─│ - products: List<Product>    │
  │ - price: double         │        │ + calcPrice(): double        │
  +─────────────────────────+        +──────────────────────────────+
                                               ▲             ▲
                                               │             │
                              +────────────────+   +─────────────────────+
                              │  InvoicePrinter│   │  <<abstract>>       │
                              +────────────────+   │    DBStorage        │
                              │+printInvoice() │   │ + saveToDB()        │
                              +────────────────+   +─────────────────────+
                                                         ▲         ▲
                                                         │         │
                                            +────────────+  +──────────────+
                                            │ MongoStorage│  │  FileStorage │
                                            +────────────+  +──────────────+
                                            │+saveToMongo │  │ +saveToFile  │
                                            +────────────+  +──────────────+
```
 
---

### ☕ Java Code

```java
// Abstract base — closed for modification ✅
abstract class DBStorage {
    ShoppingCart sc;
 
    DBStorage(ShoppingCart sc) {
        this.sc = sc;
    }
 
    abstract void save();
}
 
// Extended for Mongo — no modification to DBStorage ✅
class MongoStorage extends DBStorage {
 
    MongoStorage(ShoppingCart sc) {
        super(sc);
    }
 
    @Override
    void save() {
        System.out.println("Saving to MongoDB: " + sc.calcTotalPrice());
    }
}
 
// Extended for File — no modification to DBStorage ✅
class FileStorage extends DBStorage {
 
    FileStorage(ShoppingCart sc) {
        super(sc);
    }
 
    @Override
    void save() {
        System.out.println("Saving to File: " + sc.calcTotalPrice());
    }
}
 
// Main
public class OCPDemo {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        cart.addProduct(new Product("Apple", 1.5));
        cart.addProduct(new Product("Banana", 0.5));
 
        DBStorage mongo = new MongoStorage(cart);
        mongo.save();   // Saving to MongoDB ✅
 
        DBStorage file = new FileStorage(cart);
        file.save();    // Saving to File ✅
    }
}
```

> 💡 To add a new storage type (e.g. `CloudStorage`), just create a **new class** that extends `DBStorage` — the original class is **never touched**. That's OCP! 🎯

---

## 3️⃣ L — Liskov Substitution Principle (LSP)

- ⭐ Subclasses should be **substitutable for their base classes**.

```
                          +─────────────────+
   Client ───────────────►│   A (Base Class) │
                          +─────────────────+
                                   ▲
                                   │ extends
                          +─────────────────+
                          │  B (Sub Class)  │
                          +─────────────────+
```

> 💡 Wherever the **Client uses A (Base Class)**, it should be able to use **B (Sub Class)** instead — without any issues or unexpected behaviour. 🎯
 
---

---

### 🔍 How Substitution Works — From the Diagram

```
   Client
     │
     └──► randomMethod(A *a)
               │
               ├── a → m1()   ← calls base class methods
               ├── a → m2()
               └── a → m3()
```

- `A` (Base Class) has: `m1()`, `m2()`, `m3()`
- `B` (Sub Class) has: `m1()`, `m2()`, `m3()`, `m4()`, `m5()` ← extends, never removes

```
  +──────────────────+          +───────────────────────────+
  │   A (Base)       │          │        B (Sub)            │
  +──────────────────+          +───────────────────────────+
  │ + m1()           │          │ + m1()  ← overrides ✅    │
  │ + m2()           │          │ + m2()  ← overrides ✅    │
  │ + m3()           │          │ + m3()  ← overrides ✅    │
  +──────────────────+          │ + m4()  ← extra ✅        │
                                │ + m5()  ← extra ✅        │
                                +───────────────────────────+
```

Since `B` has all methods of `A` and more — `A *a = new B()` works perfectly. 🎯

```java
void randomMethod(A a) {
    a.m1();   // Works whether 'a' is A or B ✅
    a.m2();   // Works whether 'a' is A or B ✅
    a.m3();   // Works whether 'a' is A or B ✅
}
 
// Client code
A a = new B();       // B substitutes A ✅
randomMethod(a);     // No surprises, no exceptions ✅
```

---

### ❌ Bad Example — Violating LSP

```java
class Bird {
    void fly() {
        System.out.println("Bird is flying.");
    }
}
 
class Penguin extends Bird {
    @Override
    void fly() {
        throw new RuntimeException("Penguins can't fly!"); // ❌ breaks substitution
    }
}
 
public class LSPDemo {
    public static void main(String[] args) {
        Bird b = new Penguin();
        b.fly(); // 💥 Throws exception — LSP violated!
    }
}
```
 
---

### ✅ Good Example — Following LSP

```java
// Base class only defines what ALL birds can do
class Bird {
    void eat() {
        System.out.println("Bird is eating.");
    }
}
 
// Flying birds extend with fly()
class FlyingBird extends Bird {
    void fly() {
        System.out.println("Bird is flying.");
    }
}
 
class Sparrow extends FlyingBird {
    // Can fly ✅
}
 
class Penguin extends Bird {
    // Cannot fly — and that's fine, it doesn't extend FlyingBird ✅
}
 
public class LSPDemo {
    public static void main(String[] args) {
        FlyingBird sparrow = new Sparrow();
        sparrow.fly();  // Works perfectly ✅
 
        Bird penguin = new Penguin();
        penguin.eat();  // Works perfectly ✅
    }
}
```

> 💡 Now **every subclass can fully substitute its parent** without breaking behaviour. LSP maintained! 🎯