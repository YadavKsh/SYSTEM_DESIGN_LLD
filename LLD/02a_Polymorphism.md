# 🔥 Types of Polymorphism in Java

---

## 1️⃣ Compile-time (Static) Polymorphism 🔧

- 🕐 **Method resolution** happens at **compile time**
- ✅ Achieved using **method overloading** or **operator overloading**

---

### 🐧 Method Overloading

> When there are **multiple functions** with the **same name** but **different parameters**, then the functions are said to be overloaded — hence known as **Function or Method Overloading**.

📌 **Functions can be overloaded by:**
- Changing the **number of arguments**, or
- Changing the **type of arguments**

#### 🔑 Key Features:
- ✅ Method resolution happens at **compile time**
- ✅ Provides **better readability** and **cleaner code** by allowing methods with the same name to perform similar actions

---

### 💻 Example 1 — Different Number of Parameters

```java
class Vehicle {
    // Method to start a vehicle with basic information
    void start(String vehicleType) {
        System.out.println("Starting a " + vehicleType);
    }

    // Overloaded method to start a vehicle with extra information
    void start(String vehicleType, int speed) {
        System.out.println("Starting a " + vehicleType + " with speed: " + speed + " km/h");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle vehicle = new Vehicle();

        // Calls method with one argument
        vehicle.start("Car");

        // Calls overloaded method with two arguments
        vehicle.start("Truck", 120);
    }
}
```

---

### 💻 Example 2 — Different Types of Parameters

```java
class Vehicle {
    // Method to start a vehicle with a string parameter
    void start(String vehicleType) {
        System.out.println("Starting a " + vehicleType);
    }

    // Overloaded method to start a vehicle with an integer parameter
    void start(int vehicleId) {
        System.out.println("Starting a vehicle with ID: " + vehicleId);
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle vehicle = new Vehicle();

        // Calls method with a string argument
        vehicle.start("Truck");

        // Calls overloaded method with an integer argument
        vehicle.start(101);
    }
}
```

---

## 2️⃣ Runtime (Dynamic) Polymorphism ⏳

- 🕐 **Method resolution** happens at **runtime** based on the **actual object type**
- ✅ Achieved through **method overriding** — closely tied to **inheritance**

---

### 🔁 Method Overriding

> **Method overriding** allows a subclass to provide a **specific implementation** for a method **already defined in its parent class**.
> The overridden method in the subclass has the **same name**, **return type**, and **parameters** as the method in the parent class.

#### 🔑 Key Features:
- ✅ Method resolution happens at **runtime** based on the **actual object type**
- ✅ Supports **dynamic method dispatch** — enables the **JVM** to determine the appropriate method implementation

---

### 💻 Example — Method Overriding with Inheritance

```java
// Parent class
class Vehicle {
    void start() {
        System.out.println("Starting a generic vehicle");
    }
}

// Subclasses overriding the start method
class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Starting a car");
    }
}

class Bike extends Vehicle {
    @Override
    void start() {
        System.out.println("Starting a bike");
    }
}

class Truck extends Vehicle {
    @Override
    void start() {
        System.out.println("Starting a truck");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle myVehicle;

        // Assign a Car object to the Vehicle reference
        myVehicle = new Car();
        myVehicle.start(); // Output: Starting a car

        // Assign a Bike object to the Vehicle reference
        myVehicle = new Bike();
        myVehicle.start(); // Output: Starting a bike

        // Assign a Truck object to the Vehicle reference
        myVehicle = new Truck();
        myVehicle.start(); // Output: Starting a truck
    }
}
```

---

### 💻 Example — Polymorphism via Interface (Code Reusability)

```java
// Interface
interface Vehicle {
    void start(); // Abstract method
}

// Implementing classes
class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Starting the car");
    }
}

class Bike implements Vehicle {
    @Override
    public void start() {
        System.out.println("Starting the bike");
    }
}

class Truck implements Vehicle {
    @Override
    public void start() {
        System.out.println("Starting the truck");
    }
}
```

---

## 🏆 Advantages of Polymorphism

### ♻️ 1. Code Reusability
- Encourages writing **generic and reusable code** by allowing a **single interface to handle multiple types**
- The `Vehicle` interface allows reuse of a **single loop** `(for (Vehicle vehicle : vehicles))` to handle different implementations
- 📝 Not needing to run separate loops for Bike, Car, Truck, etc.

#### 💻 Example — Code Reusability via Single Loop

```java
public class Main {
    public static void main(String[] args) {
        Vehicle[] vehicles = {new Car(), new Bike(), new Truck()};

        for (Vehicle vehicle : vehicles) {
            vehicle.start(); // Polymorphic behavior
        }
    }
}
```

---

### 🔀 2. Flexibility
- Provides flexibility in program design by enabling **dynamic method behavior**
- Using the `Vehicle` interface, we can **dynamically switch** between different implementations (Car, Bike, Truck) **at runtime**

#### 💻 Example — Flexibility

```java
public class Main {
    public static void main(String[] args) {
        Vehicle vehicle;

        // Flexible: Dynamically assign different types of vehicles
        vehicle = new Car();
        vehicle.start(); // Output: Starting the car

        vehicle = new Bike();
        vehicle.start(); // Output: Starting the bike

        vehicle = new Truck();
        vehicle.start(); // Output: Starting the truck
    }
}
```

---

### 🔌 3. Extensibility
- Allows easy extension of code by **adding new classes / methods** or **overriding existing ones**
- Adding new vehicle types (e.g. `Bus`) is **seamless and requires no changes to existing code** — new classes just implement the `Vehicle` interface

#### 💻 Example — Extensibility (Adding a New Vehicle)

```java
// Adding a new type of Vehicle — no existing code changes needed!
class Bus implements Vehicle {
    @Override
    public void start() {
        System.out.println("Starting the bus");
    }
}

public class Main {
    public static void main(String[] args) {
        // Extensible: Add new vehicle types without changing existing code
        Vehicle[] vehicles = {new Car(), new Bike(), new Truck(), new Bus()};

        for (Vehicle vehicle : vehicles) {
            vehicle.start(); // Polymorphic behavior handles the new type seamlessly
        }
    }
}
```

> 🌟 **Beauty:** Just plug in the new class — the loop handles it automatically!

---

## ⚠️ Disadvantages of Polymorphism

### 🐛 1. Complex Debugging
- Runtime polymorphism can make debugging **difficult** due to **dynamic method resolution**
- When iterating through a list of `Vehicle` objects, it's unclear during debugging which specific subclass (**Car, Bike, Truck, or Vehicle**) is being called without stepping through the code or inspecting runtime variables
- ⚠️ If the list comes from an **external API, file, or database**, the actual type of each object isn't clear from the source code, making it **difficult to debug**

### ⏱️ 2. Performance Overhead *(Very, very, very little)*
- Dynamic method dispatch introduces **slight overhead** as the **JVM resolves the method during runtime**

---

## 📊 Quick Comparison Table

| Feature | Compile-time (Static) | Runtime (Dynamic) |
|---|---|---|
| Resolution Time | Compile Time | Runtime |
| Mechanism | Method Overloading | Method Overriding |
| Inheritance Required? | ❌ No | ✅ Yes |
| Flexibility | Less Flexible | More Flexible |
| Example | `start(String)` vs `start(int)` | `Car.start()` overrides `Vehicle.start()` |

---

## ✅ Conclusion

- 🔥 **Polymorphism** is a powerful concept in OOP that promotes **flexibility**, **modularity**, and **reusability**
- Understanding its types — **Compile-time and Runtime** — is essential for mastering Object-Oriented Programming
- By leveraging polymorphism effectively, developers can write **cleaner, maintainable, and scalable code!** 🖥️🔥

---

> 💡 **Interview Trick:** In method overloading, same method name is reused (like an overloaded truck — same truck, different cargo). In method overriding, whatever the parent has, the child just overrides it!