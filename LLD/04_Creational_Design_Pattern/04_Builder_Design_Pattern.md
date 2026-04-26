# 🏗️ Builder Design Pattern — Complete Notes

---

## 🔧 What is the Builder Design Pattern?

- 💡 **A Creational Design Pattern for Complex Objects** 🚗

The **Builder Design Pattern** is a **creational design pattern** that helps in **constructing complex objects step-by-step.** 🎯

- ✅ **Ideal when an object has many attributes or optional fields**
- ✅ Allows you to **create different configurations** of the object **easily & clearly.** 🚀

> 🧠 **My Note:** Think of the Builder Pattern like ordering a custom burger — you pick the bun, patty, toppings one by one, and only when you're done does the final burger get assembled. You don't have to specify *everything* upfront in one go.

---

## 🤔 Why Use the Builder Pattern?

🔴 Using a **constructor with many parameters** can be:

- ❌ *Cumbersome* 😵
- ❌ *Error-prone* 🔴
- ❌ *Hard to read & maintain* 📉

💡 **The Builder Pattern solves this by:**

- ✔️ Separating the **construction process** from the **final object** 🏗️
- ✔️ Letting you **build an object piece by piece** 🔄
- ✔️ Assembling the **final product only when you call** `build()` ✅

---

## ✨ In Short:

The **Builder Pattern** is a way to construct objects in a:

- ✨ **Flexible** 🎯
- ✨ **Readable** 📖
- ✨ **Maintainable** 🔧 **manner**

🚀 **Say goodbye to bloated constructors & hello to cleaner object creation!** 🚗🔥

---

## 📚 Traditional Approach

### 🚗 Revisiting the Traditional Approach: Using Constructors to Create Objects

Consider we have a **Car** class with many attributes, some of which are **optional**.

---

### 💠 Why Constructors?

Constructors were introduced to **ensure objects are created in a valid state** 🔄 right when they are **instantiated**.

They allow you to:

- ✅ **Initialize an object** with necessary values. 🚗
- ✅ **Guarantee that all required properties** are set up immediately. 🎯

---

### 🖥️ Traditional Car Class (Code)

```java
public class Car {
    private String engine;
    private int wheels;
    private int seats;
    private String color;
    private boolean sunroof;
    private boolean navigationSystem;

    // ← Constructor
    public Car(String engine, int wheels, int seats, String color, boolean sunroof,
               boolean navigationSystem) {
        this.engine = engine;
        this.wheels = wheels;
        this.seats = seats;
        this.color = color;
        this.sunroof = sunroof;
        this.navigationSystem = navigationSystem;
    }
}
```

> 🧠 **My Note:** This works fine for simple objects, but as soon as attributes grow (especially optional ones), this constructor becomes a nightmare to use and maintain.

---

## ⚠️ Drawbacks of the Traditional Approach

While this approach works, as we can see, there are **several drawbacks:**

---

### 🔴 Problem #1: Passing Unnecessary Values

- 💠 **What happens when you need to set optional attributes** (e.g., sunroof, navigation system)? 🤔
- 💠 **You must pass values for all parameters**, even if some are **not necessary!** ❌

💡 **Example:**

```java
Car car = new Car("V8", 4, 5, "Red", false, false);
//                                    ↑      ↑
//                              (HAVE TO PASS even if not needed!)
```

---

### 👀 Even if the **sunroof** and **navigation system** are **not needed**,
👉 The client **still has to pass false** for those parameters. 😤

---

### 🔧 Problem #2: Constructor Overloading & Huge Combinations

- 💠 If a car has **many optional attributes**, you end up with **multiple constructors**
- 💠 Each constructor handles **a different combination of parameters** 🔄
- 💠 This leads to **code duplication** and results in **messy, unmanageable code** 💀

> 📝 **Instructor's Note (from annotations):** For 6 parameters → 2⁶ = **64 different constructors** would be needed! 😱

💡 **Example:**

```java
public class Car {
    public Car(String engine, int wheels, int seats, String color, boolean sunroof,
               boolean navigationSystem) { ... }
    public Car(String engine, int wheels, int seats, String color) { ... }
    public Car(String engine, int wheels, int seats) { ... }
}
```

> 🧠 **My Note:** This is called the **"Telescoping Constructor" anti-pattern** — every new optional field exponentially multiplies the number of constructors you need to maintain.

---

### 📉 The more attributes you add, the more **constructor combinations** you have.

- ❌ **Constructor bloat** makes the class **harder to manage!** 😡
- 🔔 **Adding new features** (like *sports seats* or *premium sound system*)?
- 👉 You have to **add even more constructors**, making it worse! 😤

---

### 📉 Problem #3: Lack of Readability *(Who will read 400 lines of constructors?)*

- 💠 The **client code becomes hard to read** when using constructors. 👀
- 💠 **Unlabeled parameters** make it **unclear** what each value represents.

💡 **Example:**

```java
Car car = new Car("V8", 4, 5, "Red", true, false);
//                                    ↑     ↑
//              Hard to know, what is being passed??
```

---

### 🔴 At a glance, can you tell what `true` and `false` mean? 🤷

- 🔴 The client would have to **refer to documentation** just to know which value represents **sunroof** or **navigation system**. 📄 😔
- 🔴 **This isn't ideal!** ❌

---

## 🎓 Interviewer's Follow-Up Questions

An interviewer might ask:

- 💠 **What if you need to add more optional attributes?**
- 💠 **What if the object creation needs to be more flexible**, especially when dealing with large objects or more parameters?

The client realizes that as the number of attributes grows, constructors become harder to maintain. They quickly realize that **constructor overloading doesn't scale well**.

---

## 🚀 Shifting to the Builder Design Pattern

> *(Very highly used in Industry)*

### 🏗️ Why is it Named the "Builder" Pattern? 🏗️

The **Builder Design Pattern** is so named because it allows you to **build** an object step-by-step.

The **builder** is responsible for **assembling** an object, and you control the process by setting attributes one by one.

Instead of passing all parameters in a constructor, you pass only the ones you care about, and the builder takes care of the rest.

---

## 🛠️ How Does It Work?

Let's see how we can implement the Builder Pattern to create a Car with **flexibility** and **clarity**.

---

### 🚗 Car Class

```java
public class Car {
    private String engine;
    private int wheels;
    private int seats;
    private String color;
    private boolean sunroof;
    private boolean navigationSystem;

    // Car constructor should be private, ensuring it's only created through the builder
    private Car(CarBuilder builder) {
        // ← copy all the values which you have in builder
        this.engine = builder.engine;
        this.wheels = builder.wheels;
        this.seats = builder.seats;
        this.color = builder.color;
        this.sunroof = builder.sunroof;
        this.navigationSystem = builder.navigationSystem;
    }

    // Getter methods for the fields
    // No need of setters → Handled by Builder
    public String getEngine() {
        return engine;
    }
    public int getWheels() {
        return wheels;
    }
    public int getSeats() {
        return seats;
    }
    public String getColor() {
        return color;
    }
    public boolean hasSunroof() {
        return sunroof;
    }
    public boolean hasNavigationSystem() {
        return navigationSystem;
    }

    @Override
    public String toString() {
        return "Car [engine=" + engine + ", wheels=" + wheels + ", seats=" + seats +
                ", color=" + color + ", sunroof=" + sunroof + ", navigationSystem=" +
                navigationSystem + "]";
    }

    // CarBuilder nested class  ← Create builder object directly
    public static class CarBuilder {
        private String engine;
        private int wheels = 4;          // Default value
        private int seats = 5;           // Default value
        private String color = "Black";  // Default value
        private boolean sunroof = false; // Default value
        private boolean navigationSystem = false; // Default value

        // Builder methods to set attributes
        // Each method returns `this` → current builder's instance (enables method chaining)
        public CarBuilder setEngine(String engine) {
            this.engine = engine;  // usually written as: engine
            return this;
        }
        public CarBuilder setWheels(int wheels) {
            this.wheels = wheels;
            return this;
        }
        public CarBuilder setSeats(int seats) {
            this.seats = seats;
            return this;
        }
        public CarBuilder setColor(String color) {
            this.color = color;
            return this;
        }
        public CarBuilder setSunroof(boolean sunroof) {
            this.sunroof = sunroof;
            return this;
        }
        public CarBuilder setNavigationSystem(boolean navigationSystem) {
            this.navigationSystem = navigationSystem;
            return this;
        }

        // Build method to create a Car object
        // Pass current builder → Private Constructor
        public Car build() {
            return new Car(this); // Return a new Car created using the builder's values
        }
    }
}
```

> 🧠 **Key Design Decisions:**
> - The `Car` constructor is **private** — the only way to create a `Car` is through the `CarBuilder`. This enforces the pattern.
> - Each `set` method in `CarBuilder` **returns `this`** — this enables **method chaining** (fluent API style).
> - Default values are set inside `CarBuilder` — so optional fields don't need to be explicitly passed.
> - **No setters needed** on `Car` — all setting is handled by the `Builder` before `build()` is called. This makes `Car` effectively **immutable** after construction.
> - You can **easily add any new attribute in the future** to the builder without breaking existing client code. ✅

---

## 🚀 Using the Builder Pattern — Main Class

```java
public class Main {
    public static void main(String[] args) {

        // Creating a car using the Builder pattern
        Car.CarBuilder builder = new Car.CarBuilder(); // ← Give any no. of arguments

        Car car1 = builder.setEngine("V8")  // → gives a builder
                .setColor("Red")
                .setSeats(5)
                .setSunroof(true)
                .build(); // The build method returns the final product
        System.out.println(car1);

        // Creating another car with different specifications
        Car car2 = builder.setEngine("V6")
                .setColor("Blue")
                .setSeats(4)
                .build(); // Sunroof and Navigation are default
        System.out.println(car2);
    }
}
```

> 🧠 **My Note:** Notice how clean and readable this is! Each attribute is **labelled** by its setter name (`.setColor("Red")`), so there's zero ambiguity about what's being passed — a stark contrast to `new Car("V8", 4, 5, "Red", true, false)`. This is the **fluent builder** style, and it's extremely common in real-world Java code (e.g., `HttpRequest.Builder`, `AlertDialog.Builder` in Android, Lombok's `@Builder`).

---

## 📊 Summary: Traditional vs Builder Pattern

| Aspect | Traditional Constructor | Builder Pattern |
|---|---|---|
| Readability | ❌ Hard to read | ✅ Self-documenting |
| Optional fields | ❌ Must pass all params | ✅ Skip what you don't need |
| Scalability | ❌ Exponential constructors | ✅ Just add a new setter |
| Immutability | ⚠️ Needs extra care | ✅ Natural with private constructor |
| Method Chaining | ❌ Not possible | ✅ Fluent API style |

---

> 🏆 **The Builder Pattern is very highly used in the industry** — mastering it is a must for SDE interviews and real-world backend development!

---

## ❓ Why is the CarBuilder Nested in the Car Class?

### 🔒 Encapsulation 🔒

- ✅ The **CarBuilder** is *tightly related* to the **Car class**, so it's *grouped inside it.* 📦
- ✅ This makes it *clear* that the builder is *exclusively* for creating **Car objects.** 🚗

### 🔑 Access to Private Fields 🔑

- ✅ The **CarBuilder** can *directly access* **private fields** of **Car** (like engine, wheels, seats). 🚗
- ✅ This means *no need for unnecessary getters & setters.* 🚀

### 📁 Logical Grouping 📁

- ✅ By *nesting* the **CarBuilder** *inside* the **Car class**,
- ✅ We keep **both classes together**, making the code **cleaner & easier to understand.** 🎯

> 🧠 **My Note:** In Java, a `static` nested class is the standard way to implement the Builder pattern. It has access to the outer class's private members, which is what allows `Car`'s private constructor to accept the builder and copy its fields — without exposing any setters on `Car` itself.

---

## ❓ Why is the CarBuilder Static?

### 🚗 No Need for a Car Instance 🚗❌

- ✅ The **CarBuilder** *doesn't need an existing Car instance* to create a new one. 🔄
- ✅ Since it's **static**, you can use it *without creating a Car object first.* 🎯

### ⚡ Efficiency

- ✅ *Avoids unnecessary object creation*, reducing memory usage. 📉
- ✅ You *don't need to instantiate Car* just to use the builder, making it **more efficient.** 🚀

### 🔧 Simpler Usage

- ✅ The **static builder** allows clients to *create a Car object directly* using:
- 👉 ***Car.CarBuilder()*** instead of needing a separate builder instance. 🚗 💡

> 🧠 **My Note:** If `CarBuilder` were non-static (an inner class), you'd need a `Car` instance just to instantiate it — `new car.CarBuilder()` — which defeats the whole purpose since you haven't built the Car yet! Making it `static` breaks that dependency.

---

## 🗺️ UML Diagram — CarBuilder & Car Relationship

```
┌──────────────────────────────────────┐         ┌────────────────────────────────┐
│            CarBuilder                │         │             Car                │
├──────────────────────────────────────┤         ├────────────────────────────────┤
│ □ String engine                      │         │ □ String engine                │
│ □ int wheels = 4                     │         │ □ int wheels                   │
│ □ int seats = 5                      │  uses   │ □ int seats                    │
│ □ String color = "Black"             │ ------> │ □ String color                 │
│ □ boolean sunroof = false            │         │ □ boolean sunroof              │
│ □ boolean navigationSystem = false   │ builds  │ □ boolean navigationSystem     │
├──────────────────────────────────────┤ ------> ├────────────────────────────────┤
│ ● CarBuilder setEngine(String)       │         │ ● String getEngine()           │
│ ● CarBuilder setWheels(int)          │         │ ● int getWheels()              │
│ ● CarBuilder setSeats(int)           │         │ ● int getSeats()               │
│ ● CarBuilder setColor(String)        │         │ ● String getColor()            │
│ ● CarBuilder setSunroof(boolean)     │         │ ● boolean hasSunroof()         │
│ ● CarBuilder setNavigationSystem(b.) │         │ ● boolean hasNavigationSystem()│
│ ● Car build()                        │         │ ● String toString()            │
└──────────────────────────────────────┘         └────────────────────────────────┘
```

---

## 📖 Explanation — Component Breakdown

### 1️⃣ Car Class 🚗

- ✅ **Contains the attributes:**
    - 🚗 **Engine**
    - ⭕ **Wheels**
    - 🪑 **Seats**
    - 🎨 **Color**
    - 🌞 **Sunroof**
    - 🧭 **Navigation System**
- ✅ **Includes methods** to **retrieve** these attributes.
- ✅ Uses a **private constructor** 🔒, ensuring that **Car objects can only be created through the CarBuilder**.

---

### 2️⃣ CarBuilder Class 🏗️

- ✅ **Has the same attributes as the Car class** but they are **mutable** 🔄.
- ✅ **Allows setting attributes** using **builder methods** 🔧.
- ✅ **Uses the build() method** to create a **Car object** by **passing the builder** as a parameter to the **Car constructor**.

---

### 3️⃣ Relationships 🔗

- ✅ The **CarBuilder** is used by **Car** to **construct a Car object.** 🚗
- ✅ The **CarBuilder class** returns a **Car instance** using the **build() method**.
- ✅ The **CarBuilder class is nested inside the Car class** for a **structured, clean approach.** 🏗️

---

## 💡 Solving the Follow-Up Questions

### ❓ What if we only want to set some attributes?

- ✅ With the **Builder Pattern**, you can **set only the attributes you care about.** 🎯
- ✅ The **remaining attributes take default values.** 🔄

💡 **Example:**
If the client **doesn't care about the** 🌞 **sunroof** or 🧭 **navigation system**, they can **skip those methods**.
👉 The **Car object** will still be created **with default values** for those fields. 🚗

---

### ❓ What if I want to add new attributes in the future?

- ✅ The **Builder Pattern makes this easy!** 🎯
- ✅ Simply **add a new setter method** to the CarBuilder class. 🔧
- ✅ No need to **modify client code** or **change the existing builder methods!**

💡 **Example:**
Want to add a **"sportsSeats"** feature? 🚗
👉 Just **add one line** in the **CarBuilder class**, and the **client doesn't need to modify their code!** 🔥

> 🧠 **My Note:** This is the **Open/Closed Principle** in action — the `CarBuilder` is *open for extension* (new setter) but *closed for modification* (existing client code doesn't break). This is one reason the Builder Pattern is so powerful in evolving codebases.

---

## 🌍 Real-Life Use Cases and Examples

### 🍔 1. Building Complex Meals

- ✅ **Custom meal ordering system** 🎯
- ✅ Select **burger size,** 🍔 **toppings,** 🍓 **drinks**
- ✅ **Only choose the options you care about**, making the process **cleaner & more flexible**.

---

### 📄 2. Creating Complex Documents

- ✅ When generating **reports, articles, or presentations**, different sections might vary.
- ✅ The **Builder Pattern** helps **assemble documents step-by-step**:
    - 📌 **Titles**
    - 🖼️ **Images**
    - 📊 **Tables**

---

### 👤 3. User Profile Creation

- ✅ Used in **apps with complex user registration forms**.
- ✅ Users can **set only the fields they want:**
    - 😃 **Name**
    - 🌐 **Email**
    - ⚙️ **Preferences**
- ✅ **Keeps the code clean** and allows **customization** without making it cluttered.

> 🧠 **My Note:** In the real world, you'll see the Builder Pattern heavily used in:
> - **Android** — `AlertDialog.Builder`, `Notification.Builder`
> - **Java HTTP** — `HttpRequest.Builder`
> - **Lombok** — `@Builder` annotation auto-generates the entire builder for you
> - **Spring Boot** — `MockMvcRequestBuilders`, `ResponseEntity`
> - **Test frameworks** — Building test objects/fixtures cleanly

---

## 🧠 Conclusion

- ✅ **The Builder Design Pattern** is an **excellent solution** for creating **complex objects** in a:
    - **Flexible** 🎯
    - **Clear** 🏗️
    - **Maintainable** 🔄 **way!**

- ❌ **Constructors** can become **messy and unmanageable** with **many parameters.** 😵

- ✅ The **Builder Pattern** allows you to **create objects step-by-step** ✅
- ✅ You can **set only the attributes you care about** ✅
- ✅ **Provides default values** for missing attributes ✅
- ✅ **Easy to extend** without modifying client code ✅
- ✅ **Keeps client code clean & easy to understand** ✅

💡 Whether you're **building** 🚗 **cars,** 🍔 **meals,** or 👤 **user profiles,**
The **Builder Pattern** ensures objects are **constructed in an organized, step-by-step manner!** 🏆 🚀