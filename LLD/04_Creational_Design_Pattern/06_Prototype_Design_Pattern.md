# 🧬 Prototype Design Pattern

---

## 🔍 Understanding the Prototype Design Pattern

💡 **In real life**, when you create something, you **don't always start from scratch** every time. Instead, you **clone an existing thing** and make a **few small changes**.

Imagine you're **baking cookies** 🍪 — instead of shaping each cookie by hand, you use a **cookie cutter** to make multiple cookies in the **same shape**! 🍪🍪

> ✅ **This is exactly what the Prototype Pattern does in programming!** 🚀

- It allows you to **create a new object by cloning an existing prototype** and modifying only what's needed.
- This pattern is **especially useful** when working with **similar objects**, saving **time and effort** ⏳🔄

---

## 🧩 Solving the Problem

Let's say we're **developing a video game** where players can **create custom characters**.

> 💡 This is a very common application for video games — same basic character structure can be **CLONED!!**

### 🎮 Each character has:
- ✅ **A name** 🏷️
- ✅ **Health points** ❤️
- ✅ **Attack power** ⚔️
- ✅ **Level** 🎯

However, **some players** want to create **characters that are similar** to existing ones but with **minor modifications** (e.g., a different **name** or **level**). 🤔

---

## ❌ Traditional Approach 🖥️

In the **traditional approach**, every time we need a **new character**, we:

- **Manually set all the attributes**, even if **most of them remain unchanged**.
- **Write repetitive code** just to tweak **one or two values**. 📝⚙️

### 💻 Traditional Code — The Character Class

```java
public class Character {
    private String name;
    private int health;
    private int attackPower;
    private int level;

    public Character(String name, int health, int attackPower, int level) {
        this.name = name;
        this.health = health;
        this.attackPower = attackPower;
        this.level = level;
    }

    public void showCharacterInfo() {
        System.out.println("Character [Name=" + name + ", Health=" + health + ", AttackPower=" + attackPower
                + ", Level=" + level + "]");
    }
}
```

> ⚠️ **New character needed?** → Manually set ALL the attributes every single time.

### 🏭 Traditional CharacterFactory — The Problem

```java
public class CharacterFactory {
    // Creating a new character each time with similar attributes
    public Character createCharacterWithNewName(String name) {
        // Creating a new character with the same attributes, just changing the name
        return new Character(name, 100, 50, 1); // Default attributes for simplicity
        // ↑ default attributes..
    }

    public Character createCharacterWithNewLevel(int level) {
        // Creating a new character with the same attributes, just changing the level
        return new Character("DefaultName", 100, 50, level); // Default name and attributes
        // ↑ level changed
    }

    public Character createCharacterWithNewAttackPower(int attackPower) {
        // Creating a new character with the same attributes, just changing the attack power
        return new Character("DefaultName", 100, attackPower, 1); // Default name
    }
}
```

---

## ⚠️ Issues With This Approach

### 🔁 1. Code Duplication

Every time we need a **character with a small change**, we **repeat the same code**, modifying just **one or two values**.

🔴 **Example:**
- Want a **new character with a different name?** → Write a new method.
- Need **one with a different level?** → Write another method.
- Want to **change attack power?** → Another method.

👉 This leads to **unnecessary repetition** and **bloated code**. 🤯

> 💡 *Each new variation = a new method. That's not scalable at all.*

---

### ❌ 2. Inefficient & Hard to Scale ❌❌

- If we have **hundreds of characters** with **slight variations**, we end up writing **many similar methods**.
- This makes the **system inefficient** and **hard to expand**. 🚧

🔴 **Example:**
- What if **we want to add a new property** like "armor" 🛡️?
- We would have to **modify every method** where we create characters! **That's messy!** 🪣😤

---

## 🎤 Interviewer's Follow-Up Questions

❓ **An interviewer might ask:**

- 💠 **What if we need to create many characters with similar attributes?** 🤔
- 💠 **Can we avoid writing so much repetitive code?** 📝❌
- 💠 **How do we make the system scalable without adding new methods every time we need a slight change?** 🔄✨

---

## 🚨 The Problem: Scaling Becomes Messy!

As the system **grows**, we **start noticing issues**:

- ❌ **Too many methods** just for small variations.
- ❌ **Code duplication** everywhere! 🤯
- ❌ **Harder to maintain** — Every time we **add a new feature**, we **must update multiple methods**.

### 💻 The "Ugly Code" Problem

```java
public class CharacterFactory {
    // Too many methods for every small change
    public Character createCharacterWithNewName(String name) {
        return new Character(name, 100, 50, 1);
    }

    public Character createCharacterWithNewLevel(int level) {
        return new Character("DefaultName", 100, 50, level);
    }

    public Character createCharacterWithNewAttackPower(int attackPower) {
        return new Character("DefaultName", 100, attackPower, 1);
    }

    public Character createCharacterWithNewHealth(int health) {
        return new Character("DefaultName", health, 50, 1);
    }

    // More and more methods for every possible variation...
}
```

> 📌 As you can see, this approach quickly becomes **hard to maintain** and **scalable**. We end up creating a bunch of **methods** for every small change, which makes the code harder to read and manage. 🤯

---

## 👑 The Savior — Why is it Called PROTOTYPE?

> 💡 Like a **template**

The **Prototype** pattern is named so because it allows you to create a new object by cloning an **existing prototype** and **modifying only what's necessary**. It's like using a **template** (prototype) to create multiple similar objects with small variations.

---

## 🧩 Solving the Problem — The Prototype Way

In the **Prototype Pattern**, instead of manually creating new objects each time by setting each attribute, we can **clone** an existing object (the prototype) and modify only the properties that need to change. This allows us to create similar objects quickly and efficiently.

---

## 🔬 Cloning the Character Object

### 💻 Step 1 — Make `Character` implement `Cloneable`

```java
public class Character implements Cloneable {
    private String name;
    private int health;
    private int attackPower;
    private int level;

    public Character(String name, int health, int attackPower, int level) {
        this.name = name;
        this.health = health;
        this.attackPower = attackPower;
        this.level = level;
    }

    @Override
    public Character clone() throws CloneNotSupportedException {
        return (Character) super.clone(); // Shallow copy of the character object
    }

    public void showCharacterInfo() {
        System.out.println("Character [Name=" + name + ", Health=" + health + ", AttackPower=" + attackPower
                + ", Level=" + level + "]");
    }
}
```

> ✅ **Key notes from the code:**
> - `implements Cloneable` — tells Java this class CAN be cloned.
> - The `clone()` method is **overridden** to allow cloning of the `Character` object.
> - `super.clone()` performs a **Shallow Copy** of the object.
> - ⚠️ You can also implement a **deep copy** yourself (see below).

---

## 🧠 Explanation

### 📌 Cloneable Interface 📎

The **Character** class **implements the Cloneable interface**. ✅

- This is **necessary** because Java's `Object` class provides a `clone()` method.
- However, this **method only works** if the class **explicitly implements Cloneable**.
- 💡 **Without implementing Cloneable**, calling `clone()` would **throw an exception**! 🚨

---

### 🔄 Overriding the `clone()` Method 🔁

- The `clone()` method is **overridden** to allow cloning of the **Character object**.
- The `super.clone()` method **performs a shallow copy** of the object.

#### 💡 Shallow Copy?

| | Description |
|---|---|
| ✅ | **Only the top-level properties are copied.** |
| ❌ | **If the object contains references to other objects, those references are shared** (not deeply copied). |

> In simple terms: Shallow copy copies the values, but if a field is itself an object (a reference type), both the original and the clone **point to the same object** in memory.

---

## 🔵 Shallow Copy vs Deep Copy — Code Example

```java
class Address {
    String city;

    public Address(String city) {
        this.city = city;
    }

    // Copy constructor for deep copy
    public Address(Address other) {
        this.city = other.city;
    }

    @Override
    public String toString() {
        return city;
    }
}

class Person {
    String name;
    Address address;

    public Person(String name, Address address) {
        this.name = name;
        this.address = address;
    }

    // Shallow copy constructor: address reference is copied.
    public Person(Person other, boolean deepCopy) {
        this.name = other.name;
        if (deepCopy) {
            // Deep copy: create a new Address instance.
            this.address = new Address(other.address); // ← new object created
        } else {
            // Shallow copy: just copy the reference.
            this.address = other.address; // ← same object shared
        }
    }
}
```

> 💡 **Key Insight:**
> - **Shallow copy** → Both original and clone share the **same** `Address` object. Change one, the other changes too! 😱
> - **Deep copy** → A brand **new** `Address` object is created for the clone. They are independent. ✅

---

## 🧠 Explanation (Continued)

### 🔄 The `clone()` Method — Recap

```java
@Override
public Character clone() throws CloneNotSupportedException {
    return (Character) super.clone(); // Shallow copy of the character object
}
```

> ⚠️ `super.clone()` performs a **Shallow copy** of the character object. You can also implement a **deep copy** manually if needed.

---

### 🛠️ Default Constructor 🔧

- The **constructor initializes** the character's attributes, including:
  - ✅ **Name** 🏷️
  - ✅ **Health** ❤️
  - ✅ **Attack Power** ⚔️
  - ✅ **Level** 🎯
- 📌 This **ensures** that every character starts with **default values** before being cloned and modified.

---

### 📋 `showCharacterInfo()` Method 📍

- This method **displays the character's attributes**.
- **Why is this useful?**
  - ✅ After cloning, we can **modify specific properties** while **keeping the rest unchanged**.
  - 💡 **Example:**
    - Clone a **warrior character** 🔵
    - Change the **name**
    - Keep **health, attack power, and level the same** 🎮

---

## 🏭 Cloning the Prototype in the Factory

🎭 **Cloning the Prototype in the Factory**

Now, let's see how we **use the `clone()` method** in the **Factory Pattern** to:
- ✅ **Create new characters** based on the **prototype** 🔄
- ✅ **Modify only the needed attributes** while **keeping others unchanged** 🔧

### 💻 The Prototype-based `CharacterFactory`

```java
public class CharacterFactory {
    private Character prototypeCharacter;

    // Constructor to create a prototype character (default character)
    public CharacterFactory() {
        prototypeCharacter = new Character("DefaultName", 100, 50, 1); // Default prototype character
    }

    // Create a character by cloning the prototype and changing only the required attributes
    public Character createCharacterWithNewName(String name) throws CloneNotSupportedException {
        Character clonedCharacter = prototypeCharacter.clone();   // ← clone
        clonedCharacter = new Character(name, clonedCharacter.health,
                clonedCharacter.attackPower,
                clonedCharacter.level);    // → clonedCharacter.setName(name); // can also use setters
        return clonedCharacter;
    }

    public Character createCharacterWithNewLevel(int level) throws CloneNotSupportedException {
        Character clonedCharacter = prototypeCharacter.clone();   // ← clone
        clonedCharacter = new Character(clonedCharacter.name,
                clonedCharacter.health, clonedCharacter.attackPower,
                level);    // ← change what you want
        return clonedCharacter;
    }

    public Character createCharacterWithNewAttackPower(int attackPower) throws CloneNotSupportedException {
        Character clonedCharacter = prototypeCharacter.clone();   // ← clone
        clonedCharacter = new Character(clonedCharacter.name,
                clonedCharacter.health, attackPower,    // ← change what you want
                clonedCharacter.level);
        return clonedCharacter;
    }
}
```

> 💡 **Annotations from the lecture:**
> - The `CharacterFactory` constructor creates a **default character** as the prototype.
> - Each method **clones** the prototype first, then **changes only what you want**.
> - Instead of a new constructor call, you can also use **setters** — e.g., `clonedCharacter.setName(name)`.
> - If **deep copy** was done in `clone()`, the clone is fully independent — based on the class's clone implementation.

---

## 🟢 Explanation — The Prototype Pattern in Action

### 💎 Prototype Object 🎭

In the **CharacterFactory constructor**, we create a **prototype character** that serves as the **template**.

This **prototype character** is used as the **base for creating new characters**, eliminating the need to redefine common attributes repeatedly.

💡 **Example:**

```java
prototypeCharacter = new Character("DefaultName", 100, 50, 1);
// Default prototype character
```

---

### 🔄 Cloning & Modifying 🔁

The methods `createCharacterWithNewName()`, `createCharacterWithNewLevel()`, and `createCharacterWithNewAttackPower()` all:

1. 🔵 **Clone the prototype character** using the `clone()` method. ✨
2. 🔵 **Modify only the specific attribute** that needs to change (e.g., **name, level, attack power**). 🔄
3. 🔵 **Keep all other attributes the same** to maintain consistency. 📌

💡 **Example — Creating a new character with a different name:**

```java
public Character createCharacterWithNewName(String name) throws CloneNotSupportedException {
    Character clonedCharacter = prototypeCharacter.clone();
    clonedCharacter = new Character(name,
            clonedCharacter.health, clonedCharacter.attackPower,
            clonedCharacter.level);
    return clonedCharacter;
}
```

---

## ⚡ Efficiency & Code Optimization

### ❌ Before (Without Prototypes):
- Creating a **new character from scratch** every time, leading to **code duplication** and **repetitive logic**. 🤯

### ✅ After (Using Prototypes):
- **Clone once, modify what's needed, and reuse the rest!** 🔄✨

---

## 📐 UML Class Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                      CharacterFactory                        │
├──────────────────────────────────────────────────────────────┤
│  □ prototypeCharacter: Character                             │
├──────────────────────────────────────────────────────────────┤
│  ● CharacterFactory()                                        │
│  ● createCharacterWithNewName(String name): Character        │
│  ● createCharacterWithNewLevel(int level): Character         │
│  ● createCharacterWithNewAttackPower(int attackPower):       │
│    Character                                                 │
└──────────────────────────────────────────────────────────────┘
                          |
                  clones and modifies
                          ↓
┌──────────────────────────────────────────────────────────────┐
│                        Character                             │
├──────────────────────────────────────────────────────────────┤
│  □ name: String                                              │
│  □ health: int                                               │
│  □ attackPower: int                                          │
│  □ level: int                                                │
├──────────────────────────────────────────────────────────────┤
│  ● Character(String name, int health, int attackPower,       │
│    int level)                                                │
│  ● clone(): Character  «Cloneable»                           │
│  ● showCharacterInfo()                                       │
└──────────────────────────────────────────────────────────────┘
                          △
                    implements
                          |
                  ┌───────────────┐
                  │   Cloneable   │
                  └───────────────┘
```

---

## 💡 What's Different? — Prototype vs Traditional

- **Clone the prototype**: Instead of creating new characters from scratch, we **clone** the prototype character, which already has default values. 🔄✨
- **Modify only what's necessary**: After cloning the prototype, we only modify the attributes that need to change (like name, level, or attack power). This means we don't have to **repeat** the logic for every variation. 🖥️💡
- **No code duplication**: We no longer need to write separate methods for every possible variation. We simply clone the prototype and adjust it as needed. 🪄🔧

---

## 🎤 Interviewer's Follow-Up Questions — Answered

### ❓ 1. What if we need to create many characters with similar attributes?

🚨 **Problem with the Traditional Approach:**
- ❌ We would have to **manually copy and paste code** to create each character variation.
- ❌ This is **inefficient** and **hard to maintain** as the number of variations increases. 📝⚠️

✅ **Solution with the Prototype Pattern:**
- Instead of **repeating code**, we **clone the prototype** and **modify only the necessary attributes**! 🔄✨
- The **prototype character acts as a base template**, allowing us to **quickly create multiple characters**. 🚀

💡 **Example:**
We can easily create a large number of **characters with different names, levels, or attack powers**: 💥👾

```java
CharacterFactory factory = new CharacterFactory();
Character warrior = factory.createCharacterWithNewName("Warrior");  // New characters with required changes
Character mage    = factory.createCharacterWithNewName("Mage");
Character knight  = factory.createCharacterWithNewLevel(5);
```

> 🎯 **Each time we clone the prototype, we modify only the necessary parts, making the process efficient and eliminating code duplication!** 🚀

---

### ❓ 2. Can we avoid writing so much repetitive code?

✅ **Absolutely!** That was the **main pain point** with the **traditional approach**.

- ❌ **Before:** We had to **write multiple methods** for **each small variation**, leading to **a lot of repetitive code**. 📝❌

💡 **With the Prototype Pattern:**
- We **only need one method** to **clone the prototype** and **adjust the required attributes** (like name, level, attack power, etc.). 🔄✨
- **No need** to create separate methods like:

```java
createCharacterWithBlueColor()
createCharacterWithRedColor()
createCharacterWithHighAttack()
```

- ✅ **Instead**, we just **clone the prototype and modify the properties in a single, efficient method!** 🔧
- ✅ **This significantly reduces code repetition** and makes the system **easier to manage**. 💡

---

### ❓ 3. How do we make the system scalable without adding new methods every time?

✅ **The beauty of the Prototype Pattern** is that we **don't need to add new methods** for every small change. 🔧🔄

💡 **How?**
- **New character variations?** Just **clone the prototype** and update the required attributes! 🚀
- **Need a different name?** Clone & update **only the name**. ✨
- **Need a new level?** Clone & update **only the level**. 🎮
- **Adding a new property (like armor)?**
  - Update the **prototype once** and all clones will automatically have the new default property. ⚔️🔵
  - Any character needing a **different armor** can be cloned & customized **without writing extra methods**. 💡

💡 **Example of Scalability:**

```java
CharacterFactory factory = new CharacterFactory();
Character newCharacter = factory.createCharacterWithNewAttackPower(100);
// No new method needed, just cloning and changing power
```

- 🎯 **No need to add a new method!**
- ✅ We simply **clone the prototype** and **modify only what is different**! 🔄✨
- ✅ **The system remains clean, efficient, and easy to maintain** as it grows. 🚀

---

## 👍 Advantages of the Prototype Pattern

### ✅ 1. Reduced Code Duplication 🔄 📝

- ✅ By **cloning an existing object**, we **avoid writing repetitive code** for every variation.
- ✅ Instead of **manually creating objects**, we **reuse and modify prototypes** efficiently.

---

### ✅ 2. Easier Maintenance 🔧⚙️

- ✅ If we **need to change something** about how objects are created (e.g., **adding a new attribute**),
- ✅ We **only update the prototype object**, rather than modifying multiple constructors or factories.

> 💡 **Example:** Adding an "armor" property? Update the prototype once → all clones get it automatically.

---

### ✅ 3. Scalability 📈 🔄🔄

- ✅ As we **add more variations**, we **don't need to create new methods**.
- ✅ We simply **clone and modify** the **prototype**, making it easy to scale.

---

### ✅ 4. Cleaner & More Flexible Code 🔧⚙️⚙️

- ✅ The **codebase becomes modular** and **easier to maintain** as the number of variations grows.
- ✅ This approach keeps the **code cleaner** and **more adaptable**.

---

## 🌍 Real-Life Use Cases and Examples

The **Prototype Pattern** is commonly used in various fields where objects need to be **cloned and slightly modified**.

---

### 🎮 1. Game Development

- 🔴 In games, many **characters** are based on the same **base class** but have **small variations**.
- ✅ The **Prototype Pattern** allows developers to **clone a base character** and **customize its attributes** for:
  - ✅ **Different players** 🤝
  - ✅ **Enemies** 😈
  - ✅ **NPCs (Non-Playable Characters)** 🤖

---

### 📄 2. Document Creation

- 📝 When generating **reports or documents**,
- ✅ The **Prototype Pattern** can **clone a base template** and **modify only specific sections**:
  - ✅ **Title** 📌
  - ✅ **Content** 📝
  - ✅ **Layout** 🎨

---

### 🖥️ 3. GUI Frameworks

- 🎨 In **Graphical User Interfaces**, components like:
  - ✅ **Buttons** 🔳
  - ✅ **Labels** 🏷️
  - ✅ **Text Fields** 📋
- ✅ Are often **cloned from a prototype** and **customized** based on **user preferences**.

> 💡 Think of React or Swing UI components — you define a base component once and reuse/customize it everywhere.

---

### ⚙️ 4. Configuration Settings

- 🔧 A **configuration object** with **default values** can be:
  - ✅ **Cloned** and modified for **each user or process**.
  - ✅ Ensuring **consistency** while **minimizing redundant object creation**.

> 💡 Example: A default database config object cloned per environment (dev, staging, prod) with only the connection URL changed.

---

## 🧠 Conclusion

The **Prototype Design Pattern** is a **powerful & efficient** way to:

- ✅ **Create new objects by cloning existing ones.**
- ✅ **Eliminate repetitive code** and **improve maintainability.**
- ✅ **Enhance flexibility** in object creation.

💡 Whether you're:
- ✅ **Building game characters** 🎮
- ✅ **Generating documents** 🗂️
- ✅ **Creating UI elements** 🖥️
- ✅ **Managing configuration settings** ⚙️

🚀 The **Prototype Pattern** makes **object creation faster, cleaner, and more efficient!** 😊

Now, instead of **building objects from scratch every time**,
👉 **You can simply clone a prototype and make quick changes!** How cool is that? ✨⭐

---

## 📊 Summary — Prototype Pattern at a Glance

| Aspect | Traditional Approach | Prototype Pattern |
|---|---|---|
| Object creation | Manual, set all fields | Clone existing + modify only what's needed |
| Code duplication | High ❌ | Minimal ✅ |
| Scalability | Hard to scale ❌ | Easy to scale ✅ |
| Maintenance | Update every method ❌ | Update only the prototype ✅ |
| Performance | Slower (full construction) | Faster (cloning is cheaper) ✅ |

---

## 🔑 Key Takeaways

1. 🧬 The **Prototype Pattern** allows creating new objects by **cloning** an existing one.
2. 🚫 It **avoids repetitive code** and eliminates the need to create a new factory method for every small variation.
3. ⚠️ In Java, the class must **implement `Cloneable`** and **override `clone()`**.
4. 🔍 Be aware of **Shallow vs Deep Copy** — for objects with nested references, you may need to implement a manual deep copy.
5. 📈 This pattern is especially powerful when object creation is **expensive** or when you need **many similar objects** with minor differences.
