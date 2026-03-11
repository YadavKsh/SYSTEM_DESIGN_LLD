# History of Programming 📜

## Machine Language (0/1)

```
01001001  11001001
```

### Characteristics:
- ❌ **Prone to error**
- ⏱️ **Tedious**
- ❌ **Not scalable**

---

# Assembly Level Languages ⚙️

## Example

```assembly
MOV A, 61H
```

### Characteristics:
- ❌ **Prone to error**
- ❌ **Not scalable**
- ⏱️ **Tedious**

---

# Procedural Programming 🔄

## Components:
- 📌 **Functions**
- 🔁 **Loops**
- 🧱 **Blocks** (if/else, switch)

## Approach:
→ **Do this**  
→ **Then do this**

---

# OO Programming ? 🤔

## Why OO Programming?

### Benefits:
- 🌍 **Real world Modelling**
- 🔒 **Data Security**
- 📈 **Scalable / Reusable**

# Objects → Interact ☕

> Objects interact with each other through their **characteristics** and **behaviors**.

---

## 🚗 Car — Example

### 🔶 Characteristics (Fields)

| Field     | Description                                              |
|-----------|----------------------------------------------------------|
| `engine`  | The type/power of engine (e.g. V6, Electric).            |
| `brand`   | The manufacturer of the car (e.g. Toyota, BMW).          |
| `model`   | The specific model name (e.g. Corolla, X5).              |
| `wheels`  | Number and type of wheels (e.g. 4 alloy wheels).         |

---

### 🔷 Behaviour (Functions/Methods)

| Method          | Description                                              |
|-----------------|----------------------------------------------------------|
| `start()`       | Starts the car engine and makes it ready to drive.       |
| `stop()`        | Shuts down the engine and brings the car to a halt.      |
| `gearShift()`   | Changes the gear up or down while driving.               |
| `accelerate()`  | Increases the speed of the car by applying throttle.     |
| `brake()`       | Reduces the speed or stops the car using brakes.         |

---

# Car Class — Concept to Code ☕

## 🗂️ Mapping

```
  Car                          Java Code
  ├── brand        ──────▶     String brand;
  ├── model        ──────▶     String model;
  └── isEngineOn   ──────▶     bool   isEngineOn;

  ├── start()      ──────▶     start() {
  │                                // code
  │                            }
  ├── stop()       ──────▶     stop() {
  │                                // code
  │                            }
  └── gearShift()  ──────▶     gearShift() {
                                   // code
                               }
```

---

# Car & Owner Class — Object Interaction ☕

## 🗂️ Relationship Diagram

```
  ┌─────┐
  │ Car │◀────────────────────
  └─────┘                    │
     ▲                       │ (Car object used inside Owner)
     │
  ┌───────┐
  │ Owner │
  └───────┘
```

---

## 💻 Java Classes

```java
class Car {
    String brand;
    String model;
    boolean isEngineOn;

    void start() { }

    void stop() { }
}

class Owner {
    Car car;          // Owner HAS-A Car (object reference)
    String name;

    void drive() {
        car.start();
        car.gearShift();
        // ...
    }
}
```

---

## 📌 Key Concept

> The `Owner` class holds a **reference** to a `Car` object — this is called a **HAS-A relationship** (Composition/Aggregation).
> The `Owner` can call `Car`'s methods like `car.start()` and `car.gearShift()` through that reference.

---

# Pillars of OOPs ☕

```
          Pillars of OOPs
          ├── Abstraction
          ├── Encapsulation
          ├── Inheritance
          └── Polymorphism
```

---

## 📌 Brief Overview

| Pillar            | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| 🧩 **Abstraction**    | Hides complex implementation details, exposing only essential features.     |
| 🔒 **Encapsulation**  | Bundles data and methods together, restricting direct access to internals.  |
| 🧬 **Inheritance**    | Allows a class to acquire properties and behaviours of another class.       |
| 🔄 **Polymorphism**   | Allows objects of different types to be treated through the same interface. |

---

# Abstraction ☕

## 🗂️ Diagram

```
                          ┌─────────────┐
                          │   Engine    │
  ┌───────┐    ┌─────┐◀───│   Gearbox   │  (hidden internals)
  │ Owner │───▶│ Car │
  └───────┘    └──┬──┘
                  │
                  ├── accelerate()
                  ├── brake()        ← exposed interface
                  └── gearShift()


  ┌─────┐
  │ Car │  ← Interface (what the user sees)
  └─────┘
```

---

## 📌 Key Concept

> The **Owner** only interacts with the **Car's interface** (`accelerate()`, `brake()`, `gearShift()`).
> The complex internals like **Engine** and **Gearbox** are **hidden** from the Owner.
> This is **Abstraction** — exposing only **what is necessary**, hiding **how it works**.

---

```java
// Real life car

abstract class Car {
    public abstract void startEngine();
    public abstract void shiftGear(int gear);
    public abstract void accelerate();
    public abstract void brake();
    public abstract void stopEngine();
}

class SportsCar extends Car {
    String brand;
    String model;
    boolean isEngineOn;
    int currentSpeed;
    int currentGear;

    SportsCar(String b, String m) {
        this.brand = b;
        this.model = m;
        this.isEngineOn = false;
        this.currentSpeed = 0;
        this.currentGear = 0;
    }

    @Override
    public void startEngine() {
        isEngineOn = true;
        System.out.println(brand + " " + model + ": Engine starts with a roar!");
    }

    @Override
    public void shiftGear(int gear) {
        if (!isEngineOn) {
            System.out.println(brand + " " + model + ": Engine is off! Cannot Shift Gear.");
            return;
        }
        currentGear = gear;
        System.out.println(brand + " " + model + ": Shifted to gear " + currentGear);
    }

    @Override
    public void accelerate() {
        if (!isEngineOn) {
            System.out.println(brand + " " + model + ": Engine is off! Cannot accelerate.");
            return;
        }
        currentSpeed += 20;
        System.out.println(brand + " " + model + ": Accelerating to " + currentSpeed + " km/h");
    }

    @Override
    public void brake() {
        currentSpeed -= 20;
        if (currentSpeed < 0) currentSpeed = 0;
        System.out.println(brand + " " + model + ": Braking! Speed is now " + currentSpeed + " km/h");
    }

    @Override
    public void stopEngine() {
        isEngineOn = false;
        currentGear = 0;
        currentSpeed = 0;
        System.out.println(brand + " " + model + ": Engine turned off.");
    }
}

// Main Method
public class Main {
    public static void main(String[] args) {
        Car myCar = new SportsCar("Ford", "Mustang");

        myCar.startEngine();
        myCar.shiftGear(1);
        myCar.accelerate();
        myCar.shiftGear(2);
        myCar.accelerate();
        myCar.brake();
        myCar.stopEngine();
    }
}
```

---

# Encapsulation ☕

> 💊 Encapsulation is like a **Capsule** — it wraps data and behavior together in one unit, providing **Data Security**.

---

## 🗂️ Diagram

```
  ┌─────────────────────────────┐
  │          Class Car          │
  │                             │
  │  // Variables → Characters  │
  │  // Methods   → Behaviour   │
  │                             │
  └─────────────────────────────┘
           ▲
           │
    Data Security
```

---

## 📌 Key Concept

| Element        | Represents     |
|----------------|----------------|
| `// Variables` | Characteristics (fields like `brand`, `model`) |
| `// Methods`   | Behaviour (functions like `start()`, `brake()`) |

> By bundling both inside a **class** and using **access modifiers** (`private`, `public`), encapsulation protects internal data from unintended access or modification.

---

# Access Modifiers ☕

```
  Access Modifiers
  ├── Public
  ├── Private
  └── Protected
```

---

## 📌 Overview

| Modifier      | Accessible From                                              |
|---------------|--------------------------------------------------------------|
| 🟢 `public`    | Anywhere — same class, same package, subclass, other packages |
| 🔴 `private`   | Only within the **same class**                               |
| 🟡 `protected` | Same class, same package, and **subclasses**                 |

---

```java
// Real life car - Encapsulation Example

class SportsCar {
    // Private fields - data hiding (encapsulation)
    private String brand;
    private String model;
    private boolean isEngineOn;
    private int currentSpeed;
    private int currentGear;

    // Constructor
    public SportsCar(String brand, String model) {
        this.brand = brand;
        this.model = model;
        this.isEngineOn = false;
        this.currentSpeed = 0;
        this.currentGear = 0;
    }

    // Public getters - controlled read access
    public String getBrand() {
        return brand;
    }

    public String getModel() {
        return model;
    }

    public boolean isEngineOn() {
        return isEngineOn;
    }

    public int getCurrentSpeed() {
        return currentSpeed;
    }

    public int getCurrentGear() {
        return currentGear;
    }

    // Public setters with validation - controlled write access
    public void setBrand(String brand) {
        if (brand != null && !brand.isEmpty()) {
            this.brand = brand;
        }
    }

    public void setModel(String model) {
        if (model != null && !model.isEmpty()) {
            this.model = model;
        }
    }

    // Public methods - controlled behavior
    public void startEngine() {
        isEngineOn = true;
        System.out.println(brand + " " + model + ": Engine starts with a roar!");
    }

    public void shiftGear(int gear) {
        if (!isEngineOn) {
            System.out.println(brand + " " + model + ": Engine is off! Cannot Shift Gear.");
            return;
        }
        if (gear < 0 || gear > 6) {
            System.out.println(brand + " " + model + ": Invalid gear! Must be between 0-6.");
            return;
        }
        currentGear = gear;
        System.out.println(brand + " " + model + ": Shifted to gear " + currentGear);
    }

    public void accelerate() {
        if (!isEngineOn) {
            System.out.println(brand + " " + model + ": Engine is off! Cannot accelerate.");
            return;
        }
        currentSpeed += 20;
        System.out.println(brand + " " + model + ": Accelerating to " + currentSpeed + " km/h");
    }

    public void brake() {
        currentSpeed -= 20;
        if (currentSpeed < 0) currentSpeed = 0;
        System.out.println(brand + " " + model + ": Braking! Speed is now " + currentSpeed + " km/h");
    }

    public void stopEngine() {
        isEngineOn = false;
        currentGear = 0;
        currentSpeed = 0;
        System.out.println(brand + " " + model + ": Engine turned off.");
    }
}

// Main Method
public class Main {
    public static void main(String[] args) {
        SportsCar myCar = new SportsCar("Ford", "Mustang");

        // Cannot directly access private fields
        // myCar.brand = "Toyota"; // ❌ Compile error - field is private
        
        // Must use public methods to interact with the object
        myCar.startEngine();
        myCar.shiftGear(1);
        myCar.accelerate();
        myCar.shiftGear(2);
        myCar.accelerate();
        myCar.brake();
        
        // Can read data through getters
        System.out.println("\nCar Details:");
        System.out.println("Brand: " + myCar.getBrand());
        System.out.println("Model: " + myCar.getModel());
        System.out.println("Current Speed: " + myCar.getCurrentSpeed() + " km/h");
        System.out.println("Current Gear: " + myCar.getCurrentGear());
        System.out.println("Engine On: " + myCar.isEngineOn());
        
        myCar.stopEngine();
    }
}
```