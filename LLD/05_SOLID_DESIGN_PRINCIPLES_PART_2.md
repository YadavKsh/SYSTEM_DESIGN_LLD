# 📐 LSP Rules — Signature, Property & Method

These are the **three formal rules** defined by Barbara Liskov and John Guttag that a subclass **must follow** to truly be substitutable for its base class.

```
  LSP Rules
  ├── 1️⃣ Signature Rule
  ├── 2️⃣ Property Rule
  └── 3️⃣ Method Rule
```

---

## 📖 What Do "Wider" and "Narrower" Mean?

Before diving into the rules, it's important to understand these two terms since they appear throughout.

**Wider (More General)** 🔵
A type that is **higher up** in the class hierarchy — more general, accepts more things.
```
  Object  ← wider  (accepts everything)
    │
  Animal  ← wider than Dog
    │
   Dog    ← narrower (more specific)
```

**Narrower (More Specific)** 🟢
A type that is **lower down** in the class hierarchy — more specific, accepts fewer things.

| Term | Meaning | Example |
|---|---|---|
| **Wider / Broader** | More general, higher in hierarchy | `Object`, `Animal` |
| **Narrower** | More specific, lower in hierarchy | `Dog`, `Labrador` |

> 💡 Think of it like a funnel 🔻 — **wide at the top** (Object, Animal) and **narrow at the bottom** (Dog, Labrador). The wider the type, the more things it can represent.

---

## 1️⃣ Signature Rule ✍️

> The subclass must have **compatible method signatures** with the base class.

This covers two things:

### 📥 Argument Types — Contravariance
The subclass method can accept the **same or wider (more general)** argument types — never narrower.

```java
class Base {
    void process(String s) { }    // accepts String
}

class Good extends Base {
    @Override
    void process(String s) { }    // same type ✅
}

// ❌ Narrower type — violates Signature Rule
class Bad extends Base {
    @Override
    void process(Integer i) { }   // completely different type ❌
}
```

### 📤 Return Types — Covariance
The subclass method can return the **same or narrower (more specific)** type — never wider.

```java
class Base {
    Animal getAnimal() { return new Animal(); }
}

class Good extends Base {
    @Override
    Dog getAnimal() { return new Dog(); }  // Dog is a Animal — narrower ✅
}

// ❌ Wider return type — violates Signature Rule
class Bad extends Base {
    @Override
    Object getAnimal() { return new Object(); }  // too wide ❌
}
```

> 💡 **Simple rule** — Arguments go **wider** (contravariance), return types go **narrower** (covariance). 🎯

### 🚨 Exception Types — Covariance
The subclass method can throw the **same or narrower (more specific)** exceptions — never new or broader ones.

```java
class Base {
    void process() throws IOException { }   // throws IOException
}

// ✅ Narrower exception — FileNotFoundException is a subclass of IOException
class Good extends Base {
    @Override
    void process() throws FileNotFoundException { }  // narrower ✅
}

// ❌ Broader exception — Exception is wider than IOException
class Bad extends Base {
    @Override
    void process() throws Exception { }   // too wide ❌
}

// ❌ Completely new exception — violates Signature Rule
class AlsoBad extends Base {
    @Override
    void process() throws SQLException { }  // unrelated exception ❌
}
```

> 💡 The caller of the base class only knows how to handle `IOException`. If the subclass throws `Exception` or `SQLException`, the caller is **caught off guard** — substitution breaks. 🎯

---

## 2️⃣ Property Rule 🏠

> The subclass must **preserve the invariants** of the base class.

An **invariant** is a condition that is **always true** about an object — at all times, before and after any method call.

### Sub-rules:
- ⭐ **Class Invariants must be preserved** — if the base class guarantees something is always true, the subclass cannot break that guarantee.
- ⭐ **History Constraint** — the subclass must not allow state changes that the base class wouldn't allow.

```java
// Base class invariant: balance is ALWAYS >= 0
class BankAccount {
    protected double balance;

    void deposit(double amount) {
        balance += amount;       // balance always >= 0 ✅
    }

    double getBalance() {
        return balance;
    }
}

// ❌ Violates Property Rule — breaks the invariant
class BadAccount extends BankAccount {
    @Override
    void deposit(double amount) {
        balance -= amount;       // balance can go negative ❌
        // Invariant broken!
    }
}

// ✅ Follows Property Rule — invariant preserved
class SavingsAccount extends BankAccount {
    @Override
    void deposit(double amount) {
        if (amount > 0) balance += amount;  // balance still always >= 0 ✅
    }
}
```

> 💡 If the base class says **"X will always be true"**, the subclass must **never break X**. 🎯

---

## 3️⃣ Method Rule ⚙️

> The subclass must **honour the contracts** of the base class methods — i.e., preconditions and postconditions.

### 📋 Preconditions — what must be true BEFORE a method runs
- Subclass can **keep same or weaken** preconditions — never strengthen them. ✅
- Strengthening means imposing extra requirements on the caller — that breaks substitution.

### 📋 Postconditions — what must be true AFTER a method runs
- Subclass can **keep same or strengthen** postconditions — never weaken them. ✅
- Weakening means delivering less than what was promised — that breaks substitution.

```java
// Base class contract:
// Precondition  → amount > 0
// Postcondition → balance increases by amount
class BankAccount {
    protected double balance = 1000;

    void withdraw(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Amount must be > 0");
        balance -= amount;
    }
}

// ❌ Strengthens precondition — violates Method Rule
class BadAccount extends BankAccount {
    @Override
    void withdraw(double amount) {
        // Now requires amount > 100 — stricter than base ❌
        if (amount <= 100) throw new IllegalArgumentException("Minimum withdrawal is 100");
        balance -= amount;
    }
}

// ✅ Same precondition, same postcondition — Method Rule followed
class GoodAccount extends BankAccount {
    @Override
    void withdraw(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Amount must be > 0");
        balance -= amount;  // Same behaviour ✅
        System.out.println("Withdrawn: " + amount);
    }
}
```

> 💡 **Simple rule** — Preconditions can only get **easier** (weaker), postconditions can only get **stronger**. Never the other way around. 🎯

---

## 🗂️ Summary Table

| Rule | What it governs | Subclass can... | Subclass cannot... |
|---|---|---|---|
| **Signature Rule** ✍️ | Method arguments, return types & exceptions | Accept wider args, return narrower types, throw narrower exceptions | Accept narrower args, return wider types, throw broader/new exceptions |
| **Property Rule** 🏠 | Class invariants & state | Preserve all invariants | Break or weaken any invariant |
| **Method Rule** ⚙️ | Pre & postconditions | Weaken preconditions, strengthen postconditions | Strengthen preconditions, weaken postconditions |

---

## 🔥 One-Line Memory Trick

```
Signature Rule  →  Compatible types (args wider, return narrower)
Property Rule   →  Never break what's always true
Method Rule     →  Ask less before, deliver more after
```

---

## 4️⃣ I — Interface Segregation Principle (ISP)

- ⭐ Many **client specific interfaces** are better than one **general purpose interface**.
- ⭐ A client should **not be forced to implement methods they don't need**.

---

### ❌ Bad Example — Violating ISP

One fat interface forces `Penguin` to implement `fly()` even though it can't. 🔴

```java
// One general purpose interface — too fat ❌
interface Bird {
    void fly();
    void eat();
    void swim();
}
 
// Penguin is forced to implement fly() even though it can't ❌
class Penguin implements Bird {
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!"); // ❌
    }
    public void eat() { System.out.println("Penguin eating."); }
    public void swim() { System.out.println("Penguin swimming."); }
}
```
 
---

### ✅ Good Example — Following ISP

Split into **small client-specific interfaces** — each class only implements what it actually needs. 🟢

```java
// Small, specific interfaces ✅
interface Eatable {
    void eat();
}
 
interface Flyable {
    void fly();
}
 
interface Swimmable {
    void swim();
}
 
// Sparrow can fly and eat — implements only what it needs ✅
class Sparrow implements Flyable, Eatable {
    public void fly() { System.out.println("Sparrow flying."); }
    public void eat() { System.out.println("Sparrow eating."); }
}
 
// Penguin can swim and eat — no forced fly() ✅
class Penguin implements Swimmable, Eatable {
    public void swim() { System.out.println("Penguin swimming."); }
    public void eat() { System.out.println("Penguin eating."); }
}
 
// Main
public class ISPDemo {
    public static void main(String[] args) {
        Sparrow sparrow = new Sparrow();
        sparrow.fly();   // ✅
        sparrow.eat();   // ✅
 
        Penguin penguin = new Penguin();
        penguin.swim();  // ✅
        penguin.eat();   // ✅
    }
}
```

> 💡 ISP keeps interfaces **lean and focused** — each client only depends on methods it actually uses. No unnecessary implementations, no fake method bodies. 🎯

---

## 5️⃣ D — Dependency Inversion Principle (DIP)

- ⭐ **High level modules** should not depend on **low level modules**. Both should depend on **abstraction**.

---

### 💡 What are High Level and Low Level Modules?

| | Meaning | Example |
|---|---|---|
| **High Level Module** 🏛️ | Contains the **business logic** — the "what to do" | `ShoppingCart`, `OrderService` |
| **Low Level Module** ⚙️ | Contains the **implementation details** — the "how to do it" | `MySQLStorage`, `EmailNotifier` |
| **Abstraction** 🔲 | An **interface or abstract class** that sits between them | `IStorage`, `INotifier` |
 
---

### ❌ Bad Example — Violating DIP

`ShoppingCart` (high level) directly depends on `MySQLStorage` (low level). Changing the DB breaks the cart. 🔴

```java
// Low level module
class MySQLStorage {
    void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}
 
// High level module — directly depends on low level ❌
class ShoppingCart {
    MySQLStorage storage = new MySQLStorage(); // tightly coupled ❌
 
    void checkout() {
        storage.save("cart data");
    }
}
```
 
---

### ✅ Good Example — Following DIP

Both `ShoppingCart` and `MySQLStorage` depend on the **abstraction** `IStorage`. 🟢

```
  +──────────────────+          +──────────────────+
  │  ShoppingCart    │─────────►│   <<interface>>  │
  │  (High Level)    │          │     IStorage     │
  +──────────────────+          +──────────────────+
                                         ▲
                              ┌──────────┴──────────┐
                              │                     │
                   +──────────────+       +──────────────+
                   │  MySQLStorage│       │  MongoStorage│
                   │  (Low Level) │       │  (Low Level) │
                   +──────────────+       +──────────────+
```

```java
// Abstraction — the contract both sides depend on ✅
interface IStorage {
    void save(String data);
}
 
// Low level module — depends on abstraction ✅
class MySQLStorage implements IStorage {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}
 
// Another low level module ✅
class MongoStorage implements IStorage {
    public void save(String data) {
        System.out.println("Saving to MongoDB: " + data);
    }
}
 
// High level module — depends on abstraction, not on concrete class ✅
class ShoppingCart {
    IStorage storage;
 
    ShoppingCart(IStorage storage) {  // injected from outside ✅
        this.storage = storage;
    }
 
    void checkout() {
        storage.save("cart data");
    }
}
 
// Main
public class DIPDemo {
    public static void main(String[] args) {
        IStorage mysql = new MySQLStorage();
        ShoppingCart cart1 = new ShoppingCart(mysql);
        cart1.checkout();  // Saving to MySQL ✅
 
        IStorage mongo = new MongoStorage();
        ShoppingCart cart2 = new ShoppingCart(mongo);
        cart2.checkout();  // Saving to MongoDB ✅
    }
}
```

> 💡 Now `ShoppingCart` doesn't care **which** storage is used — swap MySQL for Mongo, or anything else, without touching `ShoppingCart` at all. That's the power of depending on **abstraction**. 🎯