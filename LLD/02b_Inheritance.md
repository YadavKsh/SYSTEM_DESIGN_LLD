# 🏛️ Inheritance and Its Types in Java

---

## 📖 What is Inheritance?

- Inheritance is a **cornerstone of Object-Oriented Programming (OOP)** that facilitates **code reuse** and establishes a **hierarchical relationship** between classes.
- By **inheriting properties and methods from a parent class**, a subclass can **extend** or **override functionalities**, enabling efficient and scalable application development.
- This promotes **code reuse**, **reduces redundancy**, and **supports polymorphism**, making applications easier to develop and maintain.

---

## 🌿 Types of Inheritance

```
1. Single Inheritance
2. Multilevel Inheritance
3. Hierarchical Inheritance
4. Multiple Inheritance (via Interfaces)
5. Hybrid Inheritance (via Interfaces)
```

---

## 1️⃣ Single Inheritance

> In single inheritance, **a subclass inherits from a single parent class**. This is the simplest form of inheritance and is widely used in Java.

#### ⭐ Key Features:
- A single subclass derives from **one** superclass
- Promotes **simplicity and clarity** in the inheritance hierarchy

#### 💻 Example

```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("The dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();  // Inherited method
        dog.bark();
    }
}
```

#### 🗂️ UML Diagram
```
┌─────────────────┐
│  C  Animal      │
│─────────────────│
│ • eat(): void   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  C  Dog         │
│─────────────────│
│ • bark(): void  │
└─────────────────┘
```

---

## 2️⃣ Multilevel Inheritance

> In multilevel inheritance, **a class inherits from a parent class**, and **another class further inherits from this child class**, forming a **chain**.

#### ⭐ Key Features:
- Establishes a **chain** of inheritance
- Enables **deeper specialization** of classes

#### 💻 Example

```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Mammal extends Animal {
    void walk() {
        System.out.println("This mammal walks.");
    }
}

class Dog extends Mammal {
    void bark() {
        System.out.println("The dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();   // Inherited from Animal
        dog.walk();  // Inherited from Mammal
        dog.bark();
    }
}
```

#### 🗂️ UML Diagram
```
┌──────────────────┐
│  C  Animal       │
│──────────────────│
│ • eat(): void    │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  C  Mammal       │
│──────────────────│
│ • walk(): void   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  C  Dog          │
│──────────────────│
│ • bark(): void   │
└──────────────────┘
```

---

## 3️⃣ Hierarchical Inheritance *(Family Tree)*

> In hierarchical inheritance, **multiple subclasses inherit from a single parent class**. This allows different classes to share common properties and behaviors defined in the superclass.

#### ⭐ Key Features:
- Multiple subclasses **share common properties** from a single superclass
- Promotes **code reuse and modularity**

#### 💻 Example

```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("The dog barks.");
    }
}

class Cat extends Animal {
    void meow() {
        System.out.println("The cat meows.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();
        dog.bark();

        Cat cat = new Cat();
        cat.eat();
        cat.meow();
    }
}
```

#### 🗂️ UML Diagram
```
          ┌─────────────────┐
          │  C  Animal      │
          │─────────────────│
          │ • eat(): void   │
          └────────┬────────┘
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
┌──────────────────┐  ┌──────────────────┐
│  C  Dog          │  │  C  Cat          │
│──────────────────│  │──────────────────│
│ • bark(): void   │  │ • meow(): void   │
└──────────────────┘  └──────────────────┘
```

---

## 4️⃣ Multiple Inheritance *(via Interfaces)*

> **Java does NOT support Multiple Inheritance directly** due to the **Diamond Problem**, but it can be achieved using **interfaces**. A single class can **inherit properties from multiple interfaces**.

---

### 💎 What is the Diamond Problem?

The diamond problem arises in languages that allow multiple inheritance with classes.

Imagine a scenario where a class inherits from **two parent classes** that both have a method with the **same name**. If the child class does not override the method, it **creates ambiguity** as to which implementation the child class should inherit. This leads to **confusion and potential conflicts** in the program.

#### ❌ Problem Example (NOT supported in Java)

```java
class Animal {
    public void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Cat meows");
    }
}

// Not supported in Java ❌
public class HybridAnimal extends Dog, Cat {
    public static void main(String[] args) {
        HybridAnimal hybrid = new HybridAnimal();
        hybrid.sound(); // Ambiguity: Should it call Dog's sound() or Cat's sound()?
    }
}
```

#### 🗂️ UML — Diamond Problem
```
          ┌──────────────────┐
          │  C  Animal       │
          │ • void sound()   │
          └────────┬─────────┘
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
┌─────────────────┐  ┌─────────────────┐
│  C  Dog         │  │  C  Cat         │
│ • void sound()  │  │ • void sound()  │
└────────┬────────┘  └────────┬────────┘
         │                    │
         └──────────┬─────────┘
                    ▼
          ┌──────────────────────┐
          │  C  HybridAnimal     │  ← Ambiguity! ❌
          │ • void sound() ???   │
          └──────────────────────┘
```

---

### ✅ How Java Resolves This?

Java avoids this problem by **not allowing multiple inheritance with classes**. Instead, **Java provides interfaces as a way to achieve multiple inheritance**. When a class implements multiple interfaces, it **must provide implementations** for the methods defined in the interfaces. This eliminates ambiguity since the child class explicitly defines the behavior of inherited methods.

#### ✅ Solution Example — Multiple Inheritance via Interfaces

```java
interface Dog {
    void sound();
}

interface Cat {
    void sound();
}

public class HybridAnimal implements Dog, Cat {
    @Override
    public void sound() {
        // You define the custom logic — no ambiguity!
        System.out.println("Dog barks"); // Any custom logic as needed
        // Cat's sound can also be called if needed
    }

    public static void main(String[] args) {
        HybridAnimal hybrid = new HybridAnimal();
        hybrid.sound(); // Calls Dog's sound (as defined above)
    }
}
```

> 💡 **You provide the implementation** — no ambiguity, no conflict!

#### ⭐ Key Features of Multiple Inheritance (via Interfaces):
- Achieved using **interfaces** to avoid ambiguity caused by multiple inheritance
- Combines the benefits of **various inheritance types**

---

## 5️⃣ Hybrid Inheritance *(via Interfaces)*

> Hybrid inheritance is a **combination of more than one type of inheritance**. It can involve both single inheritance and multiple inheritance. In Java, **hybrid inheritance is achieved by combining classes and interfaces**. Since Java doesn't support multiple inheritance with classes (to avoid the diamond problem), this type can only be implemented using **interfaces alongside class inheritance**.

> 📝 Combination of: **class** (single inheritance) + **interfaces** (multiple inheritance)

#### 💻 Example

```java
class Animal {
    void eat() {
        System.out.println("The animal eats food.");
    }
}

// Interface for multiple inheritance
interface Mammal {
    void walk();
}

// Interface for multiple inheritance
interface Pet {
    void play();
}

// Hybrid inheritance: extends a class + implements multiple interfaces
class Dog extends Animal implements Mammal, Pet {
    @Override
    public void eat() {
        System.out.println("The dog eats food.");
    }

    @Override
    public void walk() {
        System.out.println("The dog walks.");
    }

    @Override
    public void play() {
        System.out.println("The dog plays.");
    }
}
```

#### 🗂️ UML Diagram
```
┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│  I  Mammal       │   │  I  Pet          │   │  C  Animal       │
│──────────────────│   │──────────────────│   │──────────────────│
│ • walk()         │   │ • play()         │   │ • void eat()     │
└────────┬─────────┘   └────────┬─────────┘   └────────┬─────────┘
         │   (interface)        │  (interface)          │ (class)
         └──────────────┬───────┘               ────────┘
                        │       extends + implements
                        ▼
              ┌──────────────────────┐
              │  C  Dog              │
              │──────────────────────│
              │ • void eat()         │
              │ • void walk()        │
              │ • void play()        │
              └──────────────────────┘
```

---

## 📊 Quick Comparison Table

| Type | Description | Java Support | Key Word |
|---|---|---|---|
| **Single** | One subclass ← One parent | ✅ Direct | `extends` |
| **Multilevel** | Chain: A ← B ← C | ✅ Direct | `extends` |
| **Hierarchical** | Many subclasses ← One parent | ✅ Direct | `extends` |
| **Multiple** | One class ← Many parents | ⚠️ Via Interfaces only | `implements` |
| **Hybrid** | Mix of above types | ⚠️ Via Interfaces + Class | `extends` + `implements` |

---

---

## 🏆 Advantages of Inheritance

### ♻️ 1. Code Reusability
- Enables **reuse of existing code**, reducing redundancy and effort
- Dog doesn't need to re-define `eat()` — it just inherits it from Animal

#### 💻 Example

```java
class Animal {
    public void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    // Inherits eat() method from Animal — Reuse!!
}

class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat(); // Reuses the eat method from Animal
    }
}
```

---

### 🔧 2. Ease of Maintenance
- **Centralizes common functionality**, making updates and bug fixes more manageable
- 📝 **Maintain at 1 place only** — fix it in the parent, it applies to all subclasses automatically
- If we need to fix a bug in `eat()` or `start()`, we only need to do it in the parent class and it will be implemented for **all subclasses**

#### 💻 Example

```java
class Animal {
    public void eat() {
        System.out.println("Animal is eating");
        // Fix a bug here once → applies to Dog AND Cat automatically!
    }
}

class Dog extends Animal {
    // Inherits eat() method from Animal
}

class Cat extends Animal {
    // Inherits eat() method from Animal
}

public class Main {
    public static void main(String[] args) {
        // If we need to fix a bug in eat() or improve it,
        // we only need to do it in Animal
        Animal animal = new Dog();
        animal.eat(); // Animal is eating

        animal = new Cat();
        animal.eat(); // Animal is eating
    }
}
```

---

### 🔌 3. Extensibility
- Allows developers to **extend functionality without altering existing code**
- You can add "Cat", "Bat", or any other Animal by simply extending Animal

> Dog and Cat both inherit the `sleep()` method from Animal. You can extend the functionality of `sleep()` in each subclass by overriding it to add specific behavior (Dog sleeping in a kennel, Cat sleeping in a tree).
>
> This is an example of **extensibility** — you don't need to modify the Animal class itself to extend its behavior for each subclass. You only need to add new or changed behavior in the subclasses as needed.
>
> Inheritance makes it easier to **add new types of animals** with different behaviors by extending Animal without changing the original Animal class.

---

### 🔀 4. Supports Polymorphism
- Facilitates **runtime polymorphism**, enabling dynamic behaviour

#### 💻 Example

```java
class Animal {
    public void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Cat meows");
    }
}

class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Dog();
        myAnimal.sound(); // Dog barks  ← runtime polymorphism!
    }
}
```

---

## ⚠️ Disadvantages of Inheritance

### 🔗 1. Increased Coupling *(But required to stop code duplication)*
- Creates a **tightly coupled relationship** between classes — changes in the superclass impact all subclasses
- ⚠️ If we **change Animal's `eat()` method**, it could **break Dog's functionality**, leading to an **Exception**
- This shows how a change in one method of the parent class can break the properties of its subclasses
- 📝 Parent classes / interfaces **can't be easily changed** once subclasses depend on them

```java
class Animal {
    public void eat() {
        System.out.println("Animal eats");
    }
}

class Dog extends Animal {
    // Inherits eat() method from Animal
}

class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat(); // Reuses the eat method from Animal
        // But if Animal.eat() changes signature or breaks → Dog breaks too!
    }
}
```

---

### 🧩 2. Complexity
- **Overuse of inheritance** can lead to overly complex and hard-to-maintain hierarchies
- 📝 LLD (Low Level Design) will make this complexity visible 😉

#### 💻 Example — Deep Nesting Problem

```java
class Animal {}
class Mammal extends Animal {}
class Dog extends Mammal {}
class Bulldog extends Dog {}

class Main {
    public static void main(String[] args) {
        Bulldog bulldog = new Bulldog();
        // Which eat()? Which walk()? Which bark() is active?
        // Very hard to trace in large systems!
    }
}
```

> Understanding this **deep nested level of inheritance structure may be difficult** in larger systems to maintain and may require refactoring to make the structure more maintainable and scalable.

---

## ✅ Conclusion

- **Inheritance** is a fundamental feature of Java that enhances **code reuse, modularity, and scalability**
- By understanding its types — **single, multilevel, hierarchical, and hybrid** — developers can design **robust and maintainable applications**
- Proper use of inheritance fosters **efficient development** while avoiding common pitfalls such as **over-coupling** and **unnecessary complexity** ✂️

---

## 📊 Quick Comparison Table

| Type | Description | Java Support | Key Word |
|---|---|---|---|
| **Single** | One subclass ← One parent | ✅ Direct | `extends` |
| **Multilevel** | Chain: A ← B ← C | ✅ Direct | `extends` |
| **Hierarchical** | Many subclasses ← One parent | ✅ Direct | `extends` |
| **Multiple** | One class ← Many parents | ⚠️ Via Interfaces only | `implements` |
| **Hybrid** | Mix of above types | ⚠️ Via Interfaces + Class | `extends` + `implements` |

---

## 🧠 Summary

- **Inheritance** is a cornerstone of OOP enabling code reuse, reduced redundancy, and polymorphism support
- Java supports **5 types** of inheritance, with Multiple and Hybrid inheritance achievable only through **interfaces**
- The **Diamond Problem** is why Java avoids direct multiple class inheritance — interfaces solve this by forcing the child class to provide its own implementation
- Multilevel → **chain**, Hierarchical → **family tree**, Hybrid → **mix of class + interfaces**
- ✅ Advantages: Code Reusability, Easy Maintenance, Extensibility, Polymorphism
- ⚠️ Disadvantages: Tight Coupling, Complexity from deep hierarchies