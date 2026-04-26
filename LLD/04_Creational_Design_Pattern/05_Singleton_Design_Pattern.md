# 👑 Singleton Design Pattern — Complete Notes

---

## 🤔 The Problem We Need to Solve

Imagine you're building a **logging system** for a **large application.** 🏗️
The goal is to have **one and only one instance** of the **logger** throughout the entire application. 🎯

- 💠 **What does this mean?**
- ✅ No matter how many **classes or threads** use the logger, they all refer to **the same object.** 🔄
- ✅ This ensures that **no multiple loggers** are created, which would **waste resources**.

> 📝 **Instructor's Note (from annotations):** This is especially relevant for things like **DB Connections** — where you may require **centralized management of resources.**

---

## 🗺️ Where You May Require Centralized Management of Resources

- → **Logging Systems**
- → **Configuration Management**
- → **DB Connections / Pools**
- → **Caching**
- → **Resource Management**

> *(Where all we need centralized management...)*

---

## 📊 What Does a Logger Do?

A **logger** writes messages to track and monitor:

- ✅ **System events** 🖥️
- ✅ **User actions** 👤
- ✅ **Errors & exceptions** ⚠️

### 💠 Example Log Messages

- ✅ **A successful login:**
    - 📝 `"User 'john_doe' logged in successfully"`
- ❌ **A failed login attempt:**
    - 🚨 `"ERROR: Invalid login attempt for user 'john_doe'"`
- ⚠️ **An exception message:**
    - 🪵 `"ERROR: NullPointerException at line 42 in UserService.java"`

---

## 🤷 Why Not Just Create a New Logger Every Time?

You might think, *"Why not create a new Logger instance every time we need it?"* 🤔

### 🔴 The Issues:

- ❌ **Increased memory usage** – Multiple instances would consume unnecessary resources. 📉
    - → File access conflicts
- ❌ **Inconsistent logging** – If different parts of the system use different loggers, logs could be **written in different places** or even **out of order.** 😱
    - → Duplicate log entries

---

## 🔍 Why Do We Need a Single Logger Instance?

Having a **single Logger instance** ensures that:

- ✅ **All logs go to the same location** (e.g., file, database, console). 📁
- ✅ **Logging remains consistent** across the entire application. 📊
- ✅ **Easier debugging & monitoring** – No need to search through multiple log files. 🔍

---

## 📚 Traditional Approach

### 🚀 The Initial Plan: Creating a Simple Logger

So, you start by creating a **simple Logger class**.
The **Logger** will handle writing messages to the **console or a log file.** 📝

```java
// → Create simple logger
public class Logger {
    public void log(String message) {
        System.out.println("Log: " + message);
    }
}
```

---

### ✅ It looks **simple enough**.
### ✅ Now, you need to **use this logger** in your application to track **important events.** 📊

---

### 🤔 The First Implementation: Creating a New Logger Instance Every Time

💡 **In your Application class**, you create a **new instance** of the Logger whenever you need it.
✅ Seems fine, right? **But wait! What's happening behind the scenes?** 🔍

```java
// ← this is an issue
// You can create instance anytime & any number of times...
public class Application {  // ← Application class
    public void run() {
        Logger logger = new Logger(); // New instance created every time
        logger.log("Application started.");
    }
}
```

---

## 🚨 The Problem: Multiple Instances of the Logger

Each time the `run()` method is executed:

- ❌ A **new Logger instance** is created.
- ❌ The application **keeps creating new loggers**, even though they all perform the **same job**.

### 🔄 Imagine More Classes Using the Logger...

Now, you also have a **UserService class** handling **user logins.** 👤
And guess what? **It ALSO creates a new Logger instance!**

```java
// Creates another new instance
public class UserService {
    public void login(String username) {
        Logger logger = new Logger();  // Another new instance created
        logger.log("User " + username + " logged in.");
    }
}
```

### 🚨 So now we have:
- ✅ A **Logger instance in the Application class** 🔄
- ✅ A **separate Logger instance in the UserService class** 🔄
- 🚨 **This means multiple loggers are running in the application!** 😵

---

## 🎓 Interviewer's Follow-Up Questions

An interviewer might ask:

- 💠 **What if you want to ensure only ONE instance of Logger exists across the entire application?** 🔑
- 💠 **How can we avoid creating multiple instances of Logger?** 🚫
- 💠 **Is this the most efficient way to handle the logging system?** ⚡

- 😡 **Right now, every part of the app is creating a new Logger object, which is inefficient!**
- 🔧 **We need to make sure that only ONE instance of the logger exists** – no matter how many times it is referenced.

---

## 😔 The Problem with the Traditional Approach: Messy & Inefficient

- 📌 **Every part of the application** (Application, UserService, etc.) creates **a new instance of the Logger**.
- 📌 This leads to **several major issues:**

### 1️⃣ Multiple Instances of Logger 📝 ⚠️

- ❌ If different parts of the system create multiple **Logger instances**, it **wastes resources.** 💸
- ❌ If logging to a **file**, each logger **might try to access & write at the same time**, leading to:
    - ❗ **Potential conflicts**
    - ❗ **Overhead**

### 2️⃣ Inconsistent Logging 🔍 📉

- ❌ With multiple loggers, you could end up with:
    - **Log messages scattered** across **different log files.** 📁
    - **Inconsistent output** in the same log file. 📝
    - **Harder debugging & monitoring.** 🔄

### 3️⃣ Difficulty Managing State 🧳 💼

- ❌ If the logger maintains **state-related data** (e.g., log file location, configuration settings):
    - Each **Logger instance could have different settings.** ⚠️
    - This creates **inconsistencies** in how logs are **managed & stored.** 😔
- 🚨 If we manually check for an existing Logger each time we need it, the code will become **even more complex!** 🤷

---

## 🚀 Time for a Better Approach!

✅ We need a **solution** where:

- 💠 **Only one Logger instance exists** across the entire application. 🔄
- 💠 **All classes use the same Logger instance** instead of creating new ones. 🏗️
- 💠 **Logging is consistent & efficient** across the system. ✅

---

## 👑 The Savior — What is the Singleton Pattern?

🌐 **What is the Singleton Pattern?**

The **Singleton Pattern** ensures that:

- ✅ A **class has only one instance** throughout the entire system. 🎯
- ✅ It provides a **global point of access** to that instance. 🌐

### 📌 Why is it called "Singleton"?

- ✔️ It guarantees that **only one object** of that class exists at any given time. 🔑

### 💠 Breaking Down the Name: "Singleton"

- 🔧 **"Single"** → **Only one instance** is created. 1️⃣
- 🔧 **"Ton"** → Ensures that **the instance is globally accessible.** 🌐
- 🚀 Just like how a singleton (a unique person) exists only once in a specific context,
- 👉 The Singleton Pattern ensures **only one object is created and used across the whole application.** 🎯

---

## 🤔 Why Do We Need Singleton?

Imagine an **important resource** (like a **Logger**, **Database Connection**, or **Configuration Manager**).

- 💡 Would you want **multiple instances** competing for access? ❌ **No!**
- ✅ The **Singleton Pattern** ensures that **all parts of the system use the same instance**, making it:
    - ✔️ **More efficient** ⚡
    - ✔️ **Easier to manage** 🔧
    - ✔️ **Consistent across the entire application** ☀️

---

## 🏗️ How Does the Singleton Work?

🏗️ **1️⃣ Private Constructor** 🚫
- 💠 **Prevents direct instantiation** from outside the class.

🏗️ **2️⃣ Static Instance** 📦
- 💠 A **static variable** holds the **single instance** of the class.

🏗️ **3️⃣ Public getInstance() Method** 🔄
- 💠 **Creates the instance only if it doesn't exist**.
- 💠 **Returns the same instance every time**.

> 🧠 **My Note:** These three pillars together form the "Lock, Store, and Serve" mechanism of the Singleton — the private constructor *locks* external creation, the static variable *stores* the one instance, and `getInstance()` *serves* it.

---

## 💻 Singleton Logger — Implementation (Example)

```java
public class Logger {
    // 1. Private static variable to hold the single instance
    private static Logger instance;

    // 2. Private constructor to prevent instantiation
    // ← No one can call this constructor from outside
    private Logger() { }

    // 3. Public static method to provide access to the instance
    // Access via class ↓
    public static Logger getLogger() {
        if (logger == null) {
            logger = new Logger(); // New instance ONLY when existing instance is NULL
        }
        return logger;
    }

    public void log(String message) {
        System.out.println("Log: " + message);
    }
}

public class Application {
    public void run() {
        Logger logger = Logger.getLogger(); // Always fetch the same instance
        // (make 1st time, if not already made)
        logger.log("Application started.");
    }
}
```

---

## 💻 Singleton Logger — Full Implementation

```java
public class Logger {
    // 1. Private static variable to hold the single instance
    private static Logger instance;

    // 2. Private constructor to prevent instantiation
    private Logger() { }

    // 3. Public method to provide access to the instance
    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger(); // Create a new instance only if it doesn't exist
        }
        return instance; // Return the existing instance
    }

    public void log(String message) {
        System.out.println("Log: " + message);
    }
}

public class Application {
    public void run() {
        // 4. Fetch the single instance of the Logger
        Logger logger = Logger.getInstance();
        logger.log("Application started.");
    }
}
```

### 📊 UML — Singleton Logger

```
┌─────────────────────────────┐
│        Application          │
├─────────────────────────────┤
│  ● run()                    │
└──────────────┬──────────────┘
               │ uses getInstance()
               ▼
┌─────────────────────────────┐
│           Logger            │
├─────────────────────────────┤
│  □ instance: Logger         │
├─────────────────────────────┤
│  ■ Logger() «private»       │  ← creates only one instance
│  ● getInstance(): Logger    │
│  ● log(String message)      │
└─────────────────────────────┘
```

> 🧠 **My Note:** This is the **"Lazy Initialization"** variant of Singleton — the instance is created only when first requested, not at class load time. The alternative is **Eager Initialization** (`private static Logger instance = new Logger();`) where the instance is created upfront. Lazy is memory-efficient but **NOT thread-safe** by default (covered separately with `synchronized`).

---

## 💡 Solving the Follow-Up Questions

Now that we've applied the **Singleton Pattern**, let's see how it helps answer some **important interview questions!** 🎯

---

### 🔑 What if we want only one Logger instance?

- ✔️ With the **Singleton Pattern**, there will **always be only one instance** of the **Logger class**, no matter **how many times** you call `getInstance()`. ☀️

---

### 🚫 How can we avoid creating multiple instances of Logger?

- ✔️ The **Singleton Pattern ensures** that **only one instance** is created.
- ✔️ Every time `getInstance()` is called, it **returns the same instance** instead of creating a new one. 🔄

> 🧠 **My Note:** In real-world Java, you'll encounter Singleton in frameworks like **Spring** (every `@Bean` is a Singleton by default in the IoC container), **Hibernate** (`SessionFactory`), and standard libraries (`Runtime.getRuntime()`). Understanding this pattern well is a **must-have** for SDE interviews.

---

### ⚡ Is this the most efficient way to handle logging?

- ✔️ **Absolutely!** The Singleton Pattern:
- ✅ Prevents **multiple object creation**, saving **memory & CPU resources.** 🧠
- ✅ **Centralizes logging**, making debugging & monitoring **easier.** 🔍
- ✅ Ensures **consistent logging** throughout the application. 📜

---

## 🌍 Real-Life Use Cases and Examples

The **Singleton Design Pattern** is used in **various real-world applications** where we need to ensure **only one instance** of an object exists.

### 1️⃣ Logging Systems 📊

- ✔️ Ensures **one Logger instance** is used across the **entire application**.
- ✔️ Keeps **log messages consistent & organized.**

### 2️⃣ Database Connections 🖥️

- ✔️ Prevents **multiple database connections**, which could lead to **inefficiency** or **resource exhaustion**.
- ✔️ Ensures **a single, shared database connection** throughout the application. 🔗

### ⚙️ 3️⃣ Configuration Settings

- ✔️ Ensures that **all configuration settings** remain **consistent** across the application.
- ✔️ Prevents different parts of the app from **using different settings**, reducing **confusion**.

### 🖼️ 4️⃣ Thread Pooling

- ✔️ A **Singleton thread pool manager** ensures **efficient resource management**.
- ✔️ Avoids **creating unnecessary threads**, which could slow down the system.

---

## 🧵 Usage of Singleton in Multithreading

📋 **Usage of Singleton in Multithreading**

Let's imagine you're working on an application that has **multiple parts**, each running on **different threads** (like a **multi-tasking kitchen** with different chefs cooking at the same time). 👨‍🍳👩‍🍳

Now, let's say one of those **parts needs to access a Logger** to write logs.

- ✅ You've **already applied the Singleton Design Pattern** to ensure that **only one instance of the Logger exists.** 👍
- 🚀 Sounds perfect, right? Well... **there's a small issue!** 😐

---

### 🔵 The Race Condition Problem

When Thread A and Thread B **both try to access** `getInstance()` at the same time, they **both enter the `getInst`ance() block simultaneously:**

```
Thread A ──┐
           ├──→ getInstance() block ──→ Two threads could create (2) instances
Thread B ──┘
```

> **RACE CONDITION** → threads compete to create the Singleton Instance

---

## 🔵 Problem in Multithreading

### 😡 Problem in Multithreading: The Chaos of Multiple Instances

Imagine this happens:

- 💠 **Thread A checks if the Logger instance is null** (it is, because no instance has been created yet). 🔍
- 💠 **Thread B does the same thing at the exact same time**, unaware that **Thread A** is also checking. 🚨
- 💠 **Both threads create a new Logger instance**, and suddenly, we have **two instances instead of one!** 😱

---

### 🚨 Why is This a Problem?

- ❌ 1️⃣ **Multiple Instances Created**
    - 💠 Now, instead of **one Logger**, we have **two or more**, leading to **inefficiency.** 🔄 📉
    - 💠 If the log messages are split between multiple loggers, **debugging becomes a nightmare!** 😨

- ⚠️ 2️⃣ **Race Conditions**
    - 💠 Threads **compete** to create the Singleton instance, leading to **unpredictable behavior**.
    - 💠 **Some logs might get lost**, or **written to different places.** 📝 🔍

---

## ✅ Solution: Making Singleton Thread-Safe

🛡️ **Solution: Making Singleton Thread-Safe**

Now, we need to **fix this issue** to ensure that:

- ✅ **Only one instance** of the Logger exists, even when multiple threads access it.
- ✅ **No race conditions** occur when creating the Singleton instance.

---

### 1️⃣ Using Synchronized Block 🔧

We can use **synchronization** to ensure that **only one thread can create the Logger instance** at a time.

🚀 **In Java**, the `synchronized` keyword is used to control **access to critical sections of code**, ensuring that **only one thread executes the block at any given time.** ⏱️

```java
public class Logger {
    private static Logger instance;
    private Logger() { }  // Private constructor to prevent instantiation

    // Synchronized method to ensure only one thread can access it at a time
    // Thread A → gets in first
    // Thread B ← has to wait
    public static synchronized Logger getInstance() {
        if (instance == null) {
            instance = new Logger();  // Only one thread will create the instance
        }
        return instance;
    }

    public void log(String message) {
        System.out.println("Log: " + message);
    }
}
```

---

### 😵 What's Happening Here?

- 💠 The `getIn`stance()` method is **synchronized**, meaning:
    - ✔️ If **Thread A** is already inside `getInstance()`, **Thread B** has to **wait**.
    - ✔️ This **prevents multiple instances** of the Logger from being created.

### 📌 Problem:

- 🚨 **Even after the instance is created**, every call to `getInstance()` **still goes through the synchronized block**, which adds **unnecessary performance overhead** and **slows down the application.** ❗

> → **SOLUTION ⇒**

---

### 2️⃣ The Double-Checked Locking Technique 🕵️ 🔵

💡 **How do we optimize this?**

👉 Instead of **synchronizing every call**, we **only synchronize when the instance is null and needs to be created.**

- ✅ This **minimizes synchronization overhead**, making the Singleton **faster and more efficient.** 🚀
    - → i.e. synchronize for the **very first time only**

---

### 🤔 What is Double-Checked Locking?

It's a **smart trick** that checks the condition **twice:**

- 1️⃣ **First Check (Outside synchronized block):**
    - If the instance **already exists**, we **return it immediately without synchronization.** ⚡
- 2️⃣ **Second Check (Inside synchronized block):**
    - If the instance is **still null**, we then **synchronize and create the instance.**
- ✅ **This ensures that only one thread creates the instance**, making it **thread-safe** while keeping it **efficient.** 🚀

---

### 💻 Double-Checked Locking — Code

```java
public class Logger {
    // volatile keyword ensures visibility across threads
    // → be visible to all threads globally
    private static volatile Logger instance;

    // Private constructor to prevent instantiation
    private Logger() { }

    public static Logger getInstance() {
        if (instance == null) {  // First check (no synchronization needed here)
            // → First time initialization
            synchronized (Logger.class) {  // Synchronize only when creating the instance
                // → Synchronize only then...
                if (instance == null) {  // Second check (inside synchronized block)
                    instance = new Logger();  // Create the instance if it's still null
                }
            }
        }
        return instance;  // Return the single instance
    }

    public void log(String message) {
        System.out.println("Log: " + message);
    }
}
```

---

### 🔍 What's Different Here?

- 💠 **The `volatile` keyword** ensures:
    - When **one thread updates the instance**, it is **immediately visible to all other threads.** 👀
    - This prevents **any thread from using an outdated version** of the Logger instance. 🔄
- 💠 **We only synchronize once** — when the instance is null and needs to be created.
    - After that, **any thread can access the already-created Logger instance without needing synchronization.**

> 🧠 **My Note:** The `volatile` keyword is critical here — without it, due to CPU caching and instruction reordering, a thread might see a partially-constructed object. `volatile` forces a happens-before guarantee: the write to `instance` completes fully before any other thread reads it.

---

### 🔬 How It Works: Step by Step

- 1️⃣ **First Check:** `getInstance()` checks if instance is already created (null check).
    - If instance is **not null**, it **immediately returns** the existing instance. ✅
- 2️⃣ **Second Check (Inside synchronized block):**
    - If instance is **still null**, we enter the **synchronized block** to **create it.** 🔒
- 3️⃣ **Efficient Access:**
    - Once the **Logger instance is created**, other threads can access it **without waiting!** 🚀
- ✅ **This makes the Singleton thread-safe** 🔵 **without the performance cost** of synchronizing every call to `getInstance()`. ⚡

---

## 📋 Summary: How We Solved the Problem

- ✅ **Only one Logger instance** is created, even in a **multithreaded environment**.
- ✅ **Threads don't block each other unnecessarily** after the instance is created.
- ✅ **The use of `volatile` ensures visibility** across all threads, avoiding race conditions.

---

## 🧠 Conclusion

### 🚀 Why Use the Singleton Pattern?

- ✔️ **Ensures only one instance exists** throughout the application. 🔄
- ✔️ **Simplifies resource management** (e.g., logging, database connections, configuration settings). 🔧
- ✔️ **Thread-safe implementation** with **Double-Checked Locking** ensures **performance & efficiency.** ⚡
- ✔️ **Widely used in real-world applications** to **reduce memory usage** and **improve efficiency.** 🚀

---

> 🏆 **Quick Recap — The 3 Singleton Variants:**
>
> | Variant | Thread-Safe? | Performance | When to Use |
> |---|---|---|---|
> | Basic (no sync) | ❌ No | ✅ Fast | Single-threaded only |
> | Synchronized method | ✅ Yes | ❌ Slow (every call locks) | Simple but inefficient |
> | Double-Checked Locking + volatile | ✅ Yes | ✅ Fast (locks once) | **Production standard** ✅ |