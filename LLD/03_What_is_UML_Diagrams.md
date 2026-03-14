# 📐 UML Diagrams in Java

## 📖 What is a UML Diagram?
**UML (Unified Modeling Language)** is a standardized visual language used to **design, document, and represent** the structure and behaviour of a software system. It acts as a blueprint before actual code is written — helping developers and teams understand the system at a glance. 🗺️

---

## 🗺️ Overview via Diagram

```
                          📐 UML Diagrams
                          /             \
                         /               \
              🏗️ Structural              🔄 Behavioural
               (Static)                  (Dynamic)
                  |                           |
           Class Diagram              Sequence Diagram
           (+ 6 more)                  (+ 6 more)
```

---

## 🏗️ Structural Diagrams (Static)
Structural diagrams represent the **static aspects** of a system — i.e., things that don't change over time. They show **what the system is made of**. 📦

### 📊 Class Diagram *(Most Important)*
The most widely used UML diagram. It represents the **classes, their attributes, methods, and the relationships** between them (inheritance, association, etc.).

> This is exactly what we drew for the `Car`, `ManualCar`, and `ElectricCar` example! 🚗

**Example:**
```
+---------------------------+
|           Car             |
+---------------------------+
| + brand: String           |
| + model: String           |
| + isEngineOn: boolean     |
| + currentSpeed: int       |
+---------------------------+
| + startEngine()           |
| + stopEngine()            |
| + accelerate()            |
| + brake()                 |
+---------------------------+
          ^             ^
          |             |
+-----------+     +--------------+
| ManualCar |     | ElectricCar  |
+-----------+     +--------------+
| currentGear|    |batteryPercent|
| shiftGear()|    |chargeBattery()|
+-----------+     +--------------+
```

### 🔐 Access Modifier Symbols in Class Diagrams
In UML Class Diagrams, access modifiers are represented by symbols placed before the field or method name:

| Symbol | Access Modifier | Meaning |
|--------|----------------|---------|
| `+` | `public` | Accessible from everywhere 🌍 |
| `-` | `private` | Accessible only within the class 🔒 |
| `#` | `protected` | Accessible within class and subclasses 🛡️ |
| `~` | `default` (package) | Accessible within the same package 📦 |

**Example with access modifiers:**
```
+-------------------------------+
|             Car               |
+-------------------------------+
| - brand: String               |
| - model: String               |
| - isEngineOn: boolean         |
| - currentSpeed: int           |
+-------------------------------+
| + startEngine(): void         |
| + stopEngine(): void          |
| + accelerate(): void          |
| + brake(): void               |
| # getSpeed(): int             |
+-------------------------------+
```

---

### 🔗 Associations in Class Diagrams
Associations define the **relationships between classes**. There are two broad types:

```
                        Associations
                        /           \
                       /             \
            Class Association     Object Association
                  |               /       |        \
            Inheritance    Simple    Aggregation  Composition
                         Association
```

#### 🧬 Class Association
**Inheritance (IS-A relationship)** — A child class inherits from a parent class. Represented by a **filled arrow** pointing to the parent.
```
ManualCar  ──────────► Car
```

#### 🔗 Object Associations
Relationships between **objects/instances** of classes.

**1. Simple Association (HAS-A — loosely)**
A class simply **uses** another class. Both can exist independently. Represented by a plain filled arrow `──>`.
```
Driver ──────────> Car
(A Driver uses a Car, but both exist independently)
```

**2. Aggregation (HAS-A — weak)**
A **"whole-part"** relationship where the part **can exist without** the whole. Represented by a **hollow diamond** `◇──`.
```
Department ◇──────── Teacher
(Department has Teachers, but Teachers exist even if Department is deleted)
```

**3. Composition (HAS-A — strong)** *(highlighted in diagram)*
A **"whole-part"** relationship where the part **cannot exist without** the whole. Represented by a **filled diamond** `◆──`.
```
Car ◆──────── Engine
(Engine cannot exist without the Car — destroyed together)
```

**☕ Java Code Example:**
```java
// Class A — the part
class Engine {
    void start() {
        System.out.println("Engine started.");
    }
}
 
// Class B — the whole (owns and creates A internally)
class Car {
    Engine engine; // HAS-A Engine
 
    // Engine is created inside Car's constructor
    // It cannot exist independently — strong ownership ◆
    Car() {
        engine = new Engine();
    }
 
    void drive() {
        System.out.println("Car is driving.");
    }
}
 
// Main
public class CompositionDemo {
    public static void main(String[] args) {
        Car car = new Car();       // Engine is created here internally
        car.drive();               // Car's own method ✅
        car.engine.start();        // Accessing Engine via Car ✅
 
        // Engine has NO existence outside of Car
        // If Car is destroyed, Engine is destroyed too 💥
    }
}
```

> 💡 Notice `engine = new Engine()` is inside `Car`'s constructor — `Car` **creates and owns** the `Engine`. This is the key marker of Composition. If `Car` is gone, `Engine` is gone too. ◆

---

### 🆚 Aggregation vs Composition

| Feature | Aggregation ◇ | Composition ◆ |
|---|---|---|
| Relationship strength | Weak (HAS-A) | Strong (HAS-A) |
| Part exists without whole? | ✅ Yes | ❌ No |
| Example | Department → Teacher | Car → Engine |
| Lifecycle dependency | Independent | Dependent |

---

### Other Structural Diagrams:
- **Object Diagram** — snapshot of objects at a specific moment.
- **Component Diagram** — shows software components and their dependencies.
- **Deployment Diagram** — maps software to hardware nodes.
- **Package Diagram** — organizes classes into packages/modules.
- **Composite Structure Diagram** — internal structure of a class.
- **Profile Diagram** — extends UML for specific domains.

---

## 🔄 Behavioural Diagrams (Dynamic)
Behavioural diagrams represent the **dynamic aspects** of a system — i.e., how the system behaves over time. They show **what the system does**. ⚙️

---

#### 🧱 Components of a Sequence Diagram

**① Participant** 👤
The actors or objects involved in the interaction. Represented as **named boxes** at the top of the diagram.
```
  +---+     +---+     +---+     +---+
  | A |     | B |     | C |     | D |
  +---+     +---+     +---+     +---+
```

**② Lifeline** ➕
A **dashed vertical line** that drops down from each participant. It represents the participant's existence over time — as long as the lifeline exists, the object is alive.
```
  +---+
  | A |
  +---+
    |
    |  ← Lifeline (dashed vertical line)
    |
    |
```

**③ Activation Bar** ▬
A **solid rectangle** drawn on top of the lifeline. It shows the period during which a participant is **actively processing** — i.e., executing a method or waiting for a response.
```
  +---+
  | A |
  +---+
    |
   ███  ← Activation Bar (active/executing)
   ███
    |
    |
```

**④ Messages** ✉️
**Arrows** drawn between lifelines to show communication between participants. There are two types:
- **Solid arrow `──►`** — method call (request sent).
- **Dashed arrow `- - ►`** — return message (response sent back).

---

**1. Synchronous Message (Sync)** 🔒
The sender **waits** for the receiver to finish and return a response before continuing. Represented by a **filled/closed arrowhead `──►`**. A dashed return arrow `- - ►` comes back to the sender.
```
  +---+               +---+
  | A |               | B |
  +---+               +---+
    |                    |
   ███──<<message>>─────►███   ← Solid filled arrow (sync call)
   ███                   ███
   ███◄─ - <<response>>- ███   ← Dashed return arrow
    |                    |
```
> 💡 `A` is **blocked** and waits until `B` responds. Only then does `A` continue. 🔒
 
---

**2. Asynchronous Message (Async)** ⚡
The sender **does NOT wait** for the receiver to respond. It fires the message and continues its own execution. Represented by an **open/half arrowhead `──>`** with no return arrow.
```
  +---+               +---+
  | A |               | B |
  +---+               +---+
    |                    |
   ███──<<message>>─────>███   ← Open arrow (async call)
   ███──<<message>>─────>███
   ███──<<message>>─────>███   ← A keeps sending without waiting
    |                    |
```
> 💡 `A` keeps executing and sending messages **without waiting** for `B` to finish. ⚡
 
---

#### 🆚 Sync vs Async Messages

| Feature | Synchronous 🔒 | Asynchronous ⚡ |
|---|---|---|
| Sender waits? | ✅ Yes — blocked | ❌ No — continues |
| Arrow type | Filled/closed `──►` | Open/half `──>` |
| Return arrow? | ✅ Yes (dashed) | ❌ No |
| Use case | Method calls, DB queries | Events, notifications, fire-and-forget |
 
---

---

**3. Create Message** 🆕
Sent to **instantiate/create a new object** during the sequence. The receiver (`B`) does **not exist at the top** — its box appears mid-diagram at the point it is created. Represented by an **open arrow `──>` with `<<create>>`**.
```
  +---+
  | A |
  +---+
    |
   ███──<<create>>──────────>+---+
    |                        | B |
    |                        +---+
    |                          |
    |                          |  ← B's lifeline starts here
    |                          |
```
> 💡 `B` didn't exist before — `A` **creates it on the fly** during execution. 🆕
 
---

**4. Destroy Message** 💥
Sent to **terminate/destroy an object** during the sequence. The receiver's lifeline ends with an **`X`** mark. Represented by an **open arrow `──>` with `<<destroy>>`**.
```
  +---+          +---+
  | A |          | B |
  +---+          +---+
    |               |
   ███──<<destroy>>─────────>X   ← B's lifeline ends here with X
    |
    |  ← A continues, B is gone 💥
    |
```
> 💡 Once `B` receives the destroy message, its lifeline terminates — it no longer participates in the sequence. 💥
 
---

**5. Lost Message** 🔴
A message that is **sent but never received** — the receiver is unknown or undefined. The arrow ends at a **filled circle `●`** instead of a participant.
```
  +---+          +---+
  | A |          | B |
  +---+          +---+
    |               |
   ███──<<lost>>───►●   ← Message ends here, never reaches B
    |               |
```
> 💡 `A` fires the message but it gets **lost in transit** — no receiver catches it. 🔴
 
---

**6. Found Message** 🟢
A message that is **received but its sender is unknown** — the origin is outside the system or undefined. The arrow starts from a **filled circle `●`** and arrives at a participant.
```
  +---+          +---+
  | A |          | B |
  +---+          +---+
    |               |
    |   ●──<<found>>►███   ← Message comes from unknown origin
    |               ███
```
> 💡 `B` receives a message but we don't know **who sent it** — it comes from outside the system boundary. 🟢
 
---

#### 🗂️ Types of Messages — Full Categorization

```
                            Messages in Sequence Diagram
                 _____________________|______________________
                /                                            \
        By Direction                                   By Lifecycle
        /          \                          /        |         |        \
      Sync        Async                   Create   Destroy    Lost      Found
      (──►)       (──>)               (<<create>>) (<<destr>>) (──►●)  (●──►)
   Waits for   Fires and             Spawns a    Terminates   Sent,    Received,
    response    forgets             new object    an object   never    unknown
                                                             received   sender
```

| Type | Arrow | Waits? | Purpose |
|---|---|---|---|
| Synchronous 🔒 | Filled `──►` + dashed return | ✅ Yes | Method call, expects response |
| Asynchronous ⚡ | Open `──>` | ❌ No | Fire-and-forget, events |
| Create 🆕 | Open `──>` with `<<create>>` | ❌ No | Instantiates a new object mid-sequence |
| Destroy 💥 | Open `──>` with `<<destroy>>` | ❌ No | Terminates an object, lifeline ends with `X` |
| Lost 🔴 | Open `──►●` | ❌ No | Sent but never received — receiver unknown |
| Found 🟢 | Open `●──►` | ❌ No | Received but sender unknown — origin outside system |
 
---

#### 🏧 Putting It All Together — ATM Withdrawal Use Case

**Use Case:** A `User` wants to withdraw cash from an ATM. Here's how the objects interact:

1. `User` calls `withdraw(amount, accNo)` on `ATM`.
2. `ATM` creates a `Transaction` object and forwards `withdraw(amount, accNo)` to it.
3. `Transaction` calls `checkAmt(amount)` on `Account` to verify sufficient balance.
4. `Account` returns `true` back to `Transaction`.
5. `Transaction` returns `true` to `ATM` and is then **destroyed** (`X`).
6. `ATM` calls `withdrawCash(amount)` on `CashDispenser` (async — dots indicate processing delay ⏳).
7. `CashDispenser` returns `amount` to `ATM`.
8. `ATM` returns `amount` back to `User`.

```
  User                        ATM                  Transaction          Account          CashDispenser
  +----+                     +----+                   +----+             +----+              +----+
  |    |                     |    |                   |    |             |    |              |    |
  +----+                     +----+                   +----+             +----+              +----+
    |                           |                        |                  |                   |
   ███──withdraw(amount,────►  ███                       |                  |                   |
   ███           accNo)        ███──withdraw(amount,───►███                 |                   |
   ███                         ███           accNo)     ███──checkAmt────► ███                  |
   ███                         ███                      ███   (amount)     ███                  |
   ███                         ███                      ███◄──return true──███                  |
   ███                         ███◄──return true────────███                 |                   |
   ███                         ███                       X                  |                   |
   ███                         ███──────────────withdrawCash(amount)──────────────────────────►███
   ███                         ███                    • • • • • (processing)                   ███
   ███                         ███◄─────────────────────────────return amount──────────────────███
   ███◄──────────return amount─███                       |                  |                   |
    |                           |                        |                  |                   |
```

> 💡 Notice how `Transaction` is a **created mid-sequence** and **destroyed** (`X`) after returning `true` — a perfect example of Create + Destroy messages in action! The dots between `ATM` and `CashDispenser` represent an **async processing delay**. 🎯

---

### Other Behavioural Diagrams:
- **Use Case Diagram** — shows interactions between users and the system.
- **Activity Diagram** — represents workflows and flowcharts.
- **State Machine Diagram** — shows states of an object and transitions.
- **Communication Diagram** — focuses on object interactions and links.
- **Interaction Overview Diagram** — combines activity and sequence diagrams.
- **Timing Diagram** — shows behaviour over a specific time period.

---

## 🆚 Structural vs Behavioural

| Feature | Structural (Static) 🏗️ | Behavioural (Dynamic) 🔄 |
|---|---|---|
| Represents | What the system **is** | What the system **does** |
| Nature | Static — doesn't change | Dynamic — changes over time |
| Key Example | Class Diagram | Sequence Diagram |
| Focus | Classes, objects, components | Interactions, flows, states |
| Used for | System design & architecture | Workflow & runtime behaviour |

---

## 🗂️ Key Takeaways

- UML is a **visual blueprint** of a software system. 📐
- **Structural diagrams** → static view → focus on **structure** (Class Diagram is the most used). 🏗️
- **Behavioural diagrams** → dynamic view → focus on **behaviour** (Sequence Diagram is the most used). 🔄
- Each category has **7 types** of diagrams.
- In OOP, the **Class Diagram** is the most important — it directly maps to your code. ✅