# 🔄 Liskov Substitution Principle with Solution

---

## 🏗️ Initial Vehicle Class Hierarchy

### 📦 Vehicle Class (Parent)

```java
public class Vehicle {
    public Integer getNumberofWheels() {
        return 2;
    }

    public Boolean hasEngine() {
        return true;
    }
}
```

### 📊 Class Hierarchy Diagram

```
                    Vehicle
                   /        \
                  /          \
            MotorCycle       Car
         (extends Vehicle)  (extends Vehicle)
                            @Override
                            getNumberofWheels()
                            { return 4; }
```

### 🏍️ MotorCycle Class

```java
public class MotorCycle extends Vehicle {
}
```

### 🚗 Car Class

```java
public class Car extends Vehicle {
    @Override
    public Integer getNumberofWheels() {
        return 4;
    }
}
```

### 💻 Main Class (Initial Implementation)

```java
public class Main {
    public static void main(String[] args) {
        
        List<Vehicle> vehicleList = new ArrayList<>();
        vehicleList.add(new MotorCycle());
        vehicleList.add(new Car());
        
        for(Vehicle vehicle : vehicleList) {
            System.out.println(vehicle.hasEngine().toString());
        }
    }
}
```

---

## ⚠️ PROBLEM

> **There is another class Bicycle also extending Vehicle.**

### 🚴 Bicycle Class (Problematic)

```java
class Bicycle extends Vehicle {
    @Override
    public Boolean hasEngine() {
        return null;
    }
}
```

### 💻 Updated Main Class

```java
public class Main {
    public static void main(String[] args) {
        
        List<Vehicle> vehicleList = new ArrayList<>();
        vehicleList.add(new MotorCycle());
        vehicleList.add(new Car());
        vehicleList.add(new Bicycle());
        
        for(Vehicle vehicle : vehicleList) {
            System.out.println(vehicle.hasEngine().toString());
        }
    }
}
```

### 🔴 RESULT

```
Exception in thread "main" java.lang.NullPointerException
```

> **❌ This violates the Liskov Substitution Principle** because replacing the parent class (Vehicle) with a child class (Bicycle) breaks the program behavior.

---

## ✅ SOLUTION

### 🎯 Improved Design

The solution is to create a proper hierarchy where `hasEngine()` is only in vehicles that actually have engines.

### 📦 Vehicle Class (Refactored)

```java
public class Vehicle {
    public Integer getNumberofWheels() {
        return 2;
    }
}
```

### 📊 Improved Class Hierarchy Diagram

```
                        Vehicle
                       /        \
                      /          \
             EngineVehicle      Bicycle
          (extends Vehicle)  (extends Vehicle)
          hasEngine() = true
                 |
                 |
         ________________
        |                |
       Car          Motorcycle
  (extends         (extends
   EngineVehicle)   EngineVehicle)
```

### 🔧 EngineVehicle Class (New Intermediate Class)

```java
class EngineVehicle extends Vehicle {
    public boolean hasEngine() {
        return true;
    }
}
```

### 🚴 Bicycle Class (Fixed)

```java
public class Bicycle extends Vehicle {
}
```

### 🚗 Car Class (Updated)

```java
public class Car extends EngineVehicle {
}
```

### 🏍️ Motorcycle Class (Updated)

```java
public class Motorcycle extends EngineVehicle {
}
```

---

## 🧪 Testing Different Approaches

### 1️⃣ First Approach - Using Vehicle (✅ Works)

```java
public class Main {
    public static void main(String[] args) {
        
        List<Vehicle> vehicleList = new ArrayList<>();
        vehicleList.add(new MotorCycle());
        vehicleList.add(new Car());
        vehicleList.add(new Bicycle());
        
        for(Vehicle vehicle : vehicleList) {
            System.out.println(vehicle.getNumberofWheels());
        }
    }
}
```

**✅ Result:** Works perfectly! All vehicles can return number of wheels.

---

### 2️⃣ Second Approach - Using Vehicle with hasEngine (❌ Compile Error)

```java
public class Main {
    public static void main(String[] args) {
        
        List<Vehicle> vehicleList = new ArrayList<>();
        vehicleList.add(new MotorCycle());
        vehicleList.add(new Car());
        vehicleList.add(new Bicycle());
        
        for(Vehicle vehicle : vehicleList) {
            System.out.println(vehicle.hasEngine());
        }
    }
}
```

**❌ Compile-Time Error**

> **Reason:** Vehicle class doesn't have `hasEngine()` method anymore.

---

### 3️⃣ Third Approach - Using EngineVehicle (❌ Compile Error)

```java
public class Main {
    public static void main(String[] args) {
        
        List<EngineVehicle> vehicleList = new ArrayList<>();
        vehicleList.add(new MotorCycle());
        vehicleList.add(new Car());
        vehicleList.add(new Bicycle());  // ⚠️ This line causes error
        
        for(EngineVehicle vehicle : vehicleList) {
            System.out.println(vehicle.hasEngine());
        }
    }
}
```

**❌ Compile-Time Error**

> **Reason:** Bicycle class doesn't extend EngineVehicle, so it cannot be added to a list of EngineVehicle.

---

## 💡 Key Takeaways

| Aspect | Before | After |
|--------|--------|-------|
| **Hierarchy** | Flat - All extend Vehicle | Segregated - EngineVehicle as intermediate |
| **hasEngine()** | In Vehicle (wrong for Bicycle) | Only in EngineVehicle |
| **LSP Compliance** | ❌ Violated | ✅ Satisfied |
| **Runtime Errors** | NullPointerException | No errors |
| **Design** | Poor - forces Bicycle to lie | Good - accurate representation |

---

## 🎯 Summary

**The Liskov Substitution Principle states:**
> If class B is a subtype of class A, then we should be able to replace object of A with B without breaking the behavior of the program.

### ✅ Correct Approach
- Create proper hierarchy based on **actual characteristics**
- Don't force child classes to override methods with `null` or throw exceptions
- Use **intermediate classes** (like `EngineVehicle`) to segregate behavior

### ❌ What to Avoid
- Making child classes return `null` for inherited methods
- Forcing all subclasses to implement methods they don't support
- Breaking the contract of the parent class

---

**🎓 Remember:** Subclass should **extend** the capability of parent class, not **narrow** it down!