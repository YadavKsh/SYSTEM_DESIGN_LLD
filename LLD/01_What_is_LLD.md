# 📚 Low Level Design (LLD)

---

## 🎯 Focus Area

LLD majorly focuses on **classes** and **objects** within a system.

![Representation of LLD](../IMAGES/LEC_1/IMG_1.jpg)

---

## 🎪 Goals of LLD

- Write **clean code**
- Ensure code is **flexible** and **maintainable**
- Make code **easy to test**

![Categories of Design Pattern](../IMAGES/LEC_1/IMG_2.jpg)

---

## 🏗️ Categories of Design Patterns

### 🔨 Creational Design Patterns

> Controls the object creation.

- **Singleton**
- **Builder**
- **Factory**
- **Abstract Factory**
- **Object Pool**
- **Prototype**

---

### 🧩 Structural Design Patterns

> Focuses on how different **classes/objects** are arranged together to solve a **larger problem** in a **flexible way**.

- **Decorator**
- **Proxy**
- **Composite**
- **Adapter**
- **Bridge**
- **Façade**
- **Flyweight**

---

### 🎭 Behavioral Design Patterns

> Focuses on how different **objects communicate and interact** with each other.

- **State**
- **Strategy**
- **Observer**
- **Chain of Responsibility**
- **Template**
- **Iterator**
- **Interpreter**
- **Command**
- **Visitor**
- **Mediator**
- **Memento**
- **Null Object**

---

## 🔗 Relationships in OOP

### ✅ is-a Relationship

Represents **inheritance**.

**Example:** A `Dog` **is-a** `Animal`.

![is-a relationship](../IMAGES/LEC_1/IMG_3.jpg)

---

### 🔹 has-a Relationship

Shows a **link between two objects**.

**Examples:**
- House **has** rooms
- Library **has** books
- School **has** students

![has-a relationship](../IMAGES/LEC_1/IMG_4.jpg)

---

### 🔄 Association

General term for **has-a relationship**.

![Association](../IMAGES/LEC_1/IMG_5.jpg)

---

## 📊 Types of Association

### 🟢 Weak Relationship (Aggregation)

> Existence of one object is **not dependent** on another.

![Aggregation](../IMAGES/LEC_1/IMG_6.jpg)

```java
public class Library {
    List<Books> books;
}
```

---

### 🔴 Strong Relationship (Composition)

> Existence of one object is **dependent** on another.

![Composition](../IMAGES/LEC_1/IMG_7.jpg)

```java
public class House {
    List<Rooms> rooms;

    public House() {
        rooms = new ArrayList<>();
        rooms.add(new Room("Living Room"));
        rooms.add(new Room("Bedroom"));
    }
}
```

---