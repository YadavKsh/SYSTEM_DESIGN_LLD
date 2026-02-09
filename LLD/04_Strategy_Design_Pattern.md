# 🎭 Strategy Design Pattern

---

## ⚠️ Initial Problem - Code Duplication

```
┌─────────────────────┐                      ┌──────────────────────┐
│  SPORTY VEHICLE     │                      │      VEHICLE         │
├─────────────────────┤      is-a            ├──────────────────────┤
│  drive() {          │─────────────────────>│   drive() {          │
│    ═══════          │                      │     ───────          │
│    ═══════          │                      │   }                  │
│    ═══════          │                      │                      │
│  }                  │                      └──────────────────────┘
└─────────────────────┘                                △
                                                       │
                                                       │ is-a
                                                       │
┌─────────────────────┐                               │
│ PASSENGER VEHICLE   │      is-a                     │
├─────────────────────┤─────────────────────────────┘
│                     │
│                     │
│                     │        ┌──────────────────────┐
└─────────────────────┘        │  OFFROAD VEHICLE     │
                               ├──────────────────────┤
                               │  drive() {           │
                               │    ═══════           │
                               │    ═══════           │
                               │    ═══════           │
                               │  }                   │
                               └──────────────────────┘
```

> **❌ Problem:** Now, suppose the `drive()` overridden by OffRoad Vehicle is same as Sporty Vehicle, then that's the **repetition of code**.
>
> Notice: Both SportyVehicle and OffRoadVehicle have the same drive() implementation (shown with red lines ═══), leading to code duplication!

---

## 🤔 Q. What is a Design Pattern?

> **💡 Definition:**
>
> A Design Pattern is a **reusable solution** to a commonly occurring problem in software design. It's not a finished piece of code you can directly insert into your program, but rather a **template or blueprint** for how to solve a problem in different situations.

---

## 🎯 Strategy Design Pattern

### 📖 Definition

> **The Strategy pattern** is a **behavioural design pattern** that lets you define a family of algorithms, encapsulate each one as a separate class, and make them interchangeable. **It allows the algorithm to vary independently from clients that use it.**

---

## 🔴 PROBLEM STATEMENT

You have a class that needs to perform a specific operation, but there are **multiple ways (algorithms)** to perform that operation. The challenge is:

1. ❌ You **don't want to hardcode** all algorithm variations into one class (violates **Single Responsibility Principle**)

2. 🔄 You want to be able to **switch between algorithms at runtime**

3. 🚫 You want to **avoid large conditional statements** (if-else or switch) to select algorithms

4. ✅ You want to **add new algorithms without modifying existing code** (Open/Closed Principle)

---

## 🚗 Drive Strategy Pattern

### 🎨 Strategy Pattern Solution

```
                    ┌────────────────────────┐
                    │  <<interface>>         │
                    │   DriveStrategy        │
                    ├────────────────────────┤
                    │   + drive()            │
                    └────────────────────────┘
                              △
                              │ implements
              ┌───────────────┼───────────────┐
              │               │               │
              │               │               │
   ┌──────────────────┐  ┌──────────────┐  ┌──────────────┐
   │  NormalDrive     │  │ SpecialDrive │  │   XYZDrive   │
   ├──────────────────┤  ├──────────────┤  ├──────────────┤
   │  + drive()       │  │ + drive()    │  │ + drive()    │
   └──────────────────┘  └──────────────┘  └──────────────┘
              △                  △
              │                  │
              │ has-a            │ has-a
              │                  │
   ┌──────────────────────────────────────────────────┐
   │                  Vehicle                         │
   ├──────────────────────────────────────────────────┤
   │  - DriveStrategy obj                             │
   │                                                   │
   │  + Vehicle(DriveStrategy obj)                    │
   │  + drive() { obj.drive(); }                      │
   └──────────────────────────────────────────────────┘
                              △
                              │ is-a
              ┌───────────────┼───────────────┐
              │               │               │
   ┌──────────────────┐  ┌──────────────┐  ┌──────────────────┐
   │ SportyVehicle    │  │ Passenger    │  │ OffRoadVehicle   │
   ├──────────────────┤  │ Vehicle      │  ├──────────────────┤
   │ SportsVehicle    │  ├──────────────┤  │ OffRoadVehicle   │
   │ () { super(new   │  │Passenger     │  │ () { super(new   │
   │  SpecialDrive    │  │Vehicle()     │  │  SpecialDrive    │
   │  Strategy()); }  │  │{ super(new   │  │  Strategy()); }  │
   │                  │  │ NormalDrive  │  │                  │
   └──────────────────┘  │ Strategy());}│  └──────────────────┘
                         └──────────────┘
```

### 🔑 Key Components

#### 1️⃣ Strategy Interface

```java
interface DriveStrategy {
    void drive();
}
```

#### 2️⃣ Concrete Strategies

**Normal Drive Strategy:**
```java
class NormalDriveStrategy implements DriveStrategy {
    @Override
    public void drive() {
        System.out.println("Normal driving capability");
    }
}
```

**Special Drive Strategy:**
```java
class SpecialDriveStrategy implements DriveStrategy {
    @Override
    public void drive() {
        System.out.println("Special driving capability - high speed!");
    }
}
```

#### 3️⃣ Context Classes (Vehicles)

**Vehicle (Base Class):**
```java
class Vehicle {
    DriveStrategy driveStrategy;

    // Constructor injection
    Vehicle(DriveStrategy driveStrategy) {
        this.driveStrategy = driveStrategy;
    }

    public void drive() {
        driveStrategy.drive();
    }
}
```

**Passenger Vehicle:**
```java
class PassengerVehicle extends Vehicle {
    PassengerVehicle() {
        super(new NormalDriveStrategy());
    }
}
```

**Sporty Vehicle:**
```java
class SportyVehicle extends Vehicle {
    SportyVehicle() {
        super(new SpecialDriveStrategy());
    }
}
```

**OffRoad Vehicle:**
```java
class OffRoadVehicle extends Vehicle {
    OffRoadVehicle() {
        super(new SpecialDriveStrategy());
    }
}
```

---

## ✨ Key Benefits

| Benefit | Description |
|---------|-------------|
| 🔄 **No Code Duplication** | SportyVehicle and OffRoadVehicle can both use SpecialDriveStrategy without repeating code |
| 🎯 **Single Responsibility** | Each strategy class has one responsibility - implementing one specific algorithm |
| 🔓 **Open/Closed Principle** | New strategies can be added without modifying existing code |
| 🔀 **Runtime Flexibility** | Can switch strategies at runtime if needed |
| 🧹 **Clean Code** | Eliminates complex if-else or switch statements |

---

## 💡 Advantages of Strategy Pattern

### ✅ **Before Strategy Pattern:**
- ❌ Code duplication across similar classes
- ❌ Difficult to add new behaviors
- ❌ Violates Single Responsibility Principle
- ❌ Hard to maintain and test

### ✅ **After Strategy Pattern:**
- ✅ Each vehicle type can have its own drive strategy without code duplication
- ✅ Easy to add new strategies without modifying existing code
- ✅ Follows SOLID principles
- ✅ Easy to maintain and test
- ✅ Algorithms are interchangeable

---

## 🎓 When to Use Strategy Pattern?

Use the Strategy pattern when:

1. 🔀 You have **multiple algorithms** for a specific task and want to switch between them
2. 🚫 You want to **avoid conditional statements** for selecting algorithms
3. ➕ You need to **add new algorithms** without changing existing code
4. 🎯 Different variants of an algorithm are needed in different contexts
5. 🔄 The algorithm needs to be **selected at runtime**

---

## 📝 Real-World Examples

### 🛒 Payment Methods
```
PaymentStrategy → CreditCardPayment, PayPalPayment, CryptoPayment
```

### 📦 Shipping Methods
```
ShippingStrategy → StandardShipping, ExpressShipping, OvernightShipping
```

### 🔐 Encryption Algorithms
```
EncryptionStrategy → AESEncryption, RSAEncryption, DESEncryption
```

### 📊 Sorting Algorithms
```
SortStrategy → BubbleSort, QuickSort, MergeSort
```

---

## 🎯 Summary

The **Strategy Design Pattern** is a powerful tool for:

- ✅ Eliminating code duplication
- ✅ Making code more flexible and maintainable
- ✅ Following SOLID principles
- ✅ Enabling runtime algorithm selection
- ✅ Simplifying testing and debugging

> **Remember:** Instead of inheriting behavior, **compose** it using strategy objects!

---

## 🔗 Related Patterns

- **State Pattern** - Similar structure but different intent (state changes vs algorithm selection)
- **Template Method Pattern** - Defines skeleton in superclass, lets subclasses override steps
- **Command Pattern** - Encapsulates requests as objects

---

**🎓 Key Takeaway:** The Strategy Pattern allows you to **define a family of algorithms**, **encapsulate each one**, and make them **interchangeable** - promoting clean, maintainable, and flexible code!