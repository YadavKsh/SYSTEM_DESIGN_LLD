# Low Level Design (LLD)

## What LLD Focuses On
LLD majorly focuses on **classes** and **objects** within a system.

<p align="center">
  <img src="IMAGES/LEC_1/IMG_1.jpg" alt="Representation of LLD" width="85%">
</p>

---

## Goals of LLD
- Write **clean code**
- Ensure the code is **flexible** and **maintainable**
- Make the code **easy to test**

<p align="center">
  <img src="IMAGES/LEC_1/IMG_2.jpg" alt="Categories of Design Pattern" width="90%">
</p>

---

## Design Pattern Categories

### Creational Patterns
Creational patterns focus on **object creation**.

**Common Creational Patterns:**
- Singleton
- Builder
- Factory
- Abstract Factory
- Object Pool
- Prototype

---

### Structural Patterns
Structural patterns focus on how different **classes / objects** are arranged so that a **larger problem** can be solved in the **most flexible way**.

**Common Structural Patterns:**
- Decorator
- Proxy
- Composite
- Adapter
- Bridge
- Façade
- Flyweight

---

### Behavioral Patterns
Behavioral patterns focus on how different **objects communicate or interact** with each other.

Once the structure (skeleton) is created using **Structural Patterns**, the **coordination, responsibility, and interaction** are governed by **Behavioral Patterns**.

**Common Behavioral Patterns:**
- State
- Strategy
- Observer
- Chain of Responsibility
- Template
- Iterator
- Interpreter
- Command
- Visitor
- Mediator
- Memento
- Null Object

---

# has-a and is-a Relationship

## is-a Relationship
- Represents **inheritance**

<p align="center">
  <img src="IMAGES/LEC_1/IMG_3.jpg" alt="is-a Relationship" width="80%">
</p>

---

## has-a Relationship
- Represents a **relationship/link between two objects**

**Examples:**
- House has rooms
- Library has books
- School has students

<p align="center">
  <img src="IMAGES/LEC_1/IMG_4.jpg" alt="has-a Relationship" width="85%">
</p>

---

## Association
- Association is a **general term** for has-a relationships

<p align="center">
  <img src="IMAGES/LEC_1/IMG_5.jpg" alt="Association Representation" width="85%">
</p>

---

# Types of Association

## Weak Relationship (Aggregation)
- **Existence of one object is NOT dependent on another**
- Example: *Library has books*

<p align="center">
  <img src="IMAGES/LEC_1/IMG_6.jpg" alt="Aggregation Example" width="80%">
</p>

**Explanation:**
- Library can exist without books
- Books can exist without the library

```java
public class Library {
  List<Books> books;
}
```

---

## Strong Relationship (Composition)

- **Existence of one object IS dependent on another**
- Example: *House has rooms*

If the **House** does not exist, the **Room** objects also do not exist.

<p align="center">
  <img src="IMAGES/LEC_1/IMG_7.jpg" alt="Composition Example" width="80%">
</p>

```java
public class House {
    List<Rooms> rooms;

    // Responsible for creating and managing Room objects
    public House() {
        rooms = new ArrayList<>();
        rooms.add(new Room("Living Room"));
        rooms.add(new Room("Bedroom"));
    }
}

```