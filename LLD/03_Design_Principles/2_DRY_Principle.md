# 🔁 DRY Principle — Don't Repeat Yourself

> *"DRY stands for Don't Repeat Yourself, and adhering to this principle can significantly improve code quality and reduce redundancy."*

**DRY** is a software development principle that encourages developers to avoid duplicating code. The main idea is to ensure that a **particular piece of knowledge or logic exists in only one place** within a codebase — creating reusable components, functions, or modules that can be utilized across different parts of the system.

---

## 🔑 Key Features of DRY

| # | Feature | Description |
|---|---------|-------------|
| 1 | **Code Reusability** | Write reusable components, functions, or modules that can be shared across multiple parts of the codebase instead of duplicating code |
| 2 | **Maintenance & Updates** | Logic exists in only one place — changes or enhancements can be made in a centralized location, reducing inconsistent updates |
| 3 | **Readability** | Eliminating unnecessary repetition makes it easier for developers to understand and navigate the codebase |
| 4 | **Consistency** | A specific functionality encapsulated in one place ensures all instances behave consistently — crucial for reliable software |
| 5 | **Reduced Development Time** | Reusing code instead of rewriting it saves significant time and effort, especially in large systems |
| 6 | **Facilitates Collaboration** | Modular, reusable code lets multiple developers work on different components without duplicating efforts |
| 7 | **Avoidance of Copy-Paste Errors** | Minimizes copy-paste by encouraging reusable units — reduces the risk of inconsistent changes introducing bugs |
| 8 | **Testability** | Specific functionalities encapsulated in distinct units are easier to test and ensure changes don't inadvertently affect unrelated parts |

---

## 🛠️ How to Implement DRY

### 1. Use Functions or Methods
Encapsulate repetitive code into reusable functions or methods.

#### ❌ Non-DRY — Duplicating swap logic

```java
public class NumberSwapper {
    // Non-DRY approach - repeating swap logic multiple places
    public void processNumbersNonDry() {
        int a = 5;
        int b = 10;
        // Swapping values here
        int temp = a;
        a = b;
        b = temp;

        // Later in the code, need to swap different numbers
        int x = 20;
        int y = 30;
        // Repeating the same swap logic ❌
        temp = x;
        x = y;
        y = temp;
    }

    // DRY approach - creating a reusable swap method ✅
    public void swap(int[] numbers) {
        if (numbers.length >= 2) {
            int temp = numbers[0];
            numbers[0] = numbers[1];
            numbers[1] = temp;
        }
    }

    public static void main(String[] args) {
        NumberSwapper swapper = new NumberSwapper();
        int[] numbers = {5, 10};
        System.out.println("Before swap: " + numbers[0] + ", " + numbers[1]);
        swapper.swap(numbers);
        System.out.println("After swap: " + numbers[0] + ", " + numbers[1]);
    }
}
```

---

### 2. Leverage Object-Oriented Principles
Use **inheritance, polymorphism, and interfaces** to share behavior between classes.

#### ❌ DRY Violation — Duplicating `onClick()` per button class

```java
// SubmitButton class with its own onClick() implementation
class SubmitButton {
    void onClick() { System.out.println("Form submitted."); }
}

// CancelButton class with its own onClick() implementation
class CancelButton {
    void onClick() { System.out.println("Action canceled."); }
}

public class Main {
    public static void main(String[] args) {
        SubmitButton submit = new SubmitButton();
        submit.onClick();  // Output: Form submitted.
        CancelButton cancel = new CancelButton();
        cancel.onClick();  // Output: Action canceled.
    }
}
```

**Problem:** Introducing new buttons requires repeating the `onClick()` implementation each time. If the button logic were complex, every new button type would need its own duplicate logic — increasing maintenance burden and inconsistency risks.

#### ✅ Adhering to DRY — Using abstract base class

```java
// Base class
abstract class Button {
    abstract void onClick();
}

// Subclass implementing specific behavior
class SubmitButton extends Button {
    @Override void onClick() {
        System.out.println("Form submitted.");
    }
}

// Subclass implementing different behavior
class CancelButton extends Button {
    @Override void onClick() {
        System.out.println("Action canceled.");
    }
}

public class Main {
    public static void main(String[] args) {
        Button submit = new SubmitButton();
        submit.onClick();  // Output: Form submitted.
        Button cancel = new CancelButton();
        cancel.onClick();  // Output: Action canceled.
    }
}
```

**Result:** New buttons simply override `onClick()` in the abstract class — no duplication of common structure.

---

### 3. Create Reusable Components
In Java, reusable components are created as methods, classes, or utility libraries to avoid duplicating similar logic or UI code.

```java
// Reusable method for button rendering
public class Button {
    private String label;

    Button(String label) { this.label = label; }

    public void render() {
        System.out.println("Rendering button: " + label);
    }
}

public class Main {
    public static void main(String[] args) {
        Button submitButton = new Button("Submit");
        submitButton.render();  // Output: Rendering button: Submit

        Button cancelButton = new Button("Cancel");
        cancelButton.render();  // Output: Rendering button: Cancel
    }
}
```

---

### 4. Use Constants and Configuration Files
Avoid hardcoding values — define them in **a single location**.

```java
// ❌ Before — hardcoded value repeated everywhere
public class Main {
    public static void main(String[] args) {
        System.out.println("Connecting to http://example.com");
    }
}

// ✅ After DRY — centralized Config class
public class Config {
    public static final String BASE_URL = "http://example.com";
}

public class Main {
    public static void main(String[] args) {
        System.out.println("Connecting to " + Config.BASE_URL);
        // Output: Connecting to http://example.com
    }
}
```

---

## ⚖️ Advantages & Disadvantages of DRY

### ✅ Advantages

| # | Advantage | Description |
|---|-----------|-------------|
| 1 | **Efficiency** | Reduces the amount of code written, saving development time |
| 2 | **Maintainability** | Changes in logic only need to be applied once — reduces risk of errors |
| 3 | **Scalability** | Modular and reusable code makes it easier to extend the application |
| 4 | **Consistency** | Ensures uniform behavior across the application |
| 5 | **Collaboration** | Clean, organized code improves teamwork and reduces onboarding time |

### ❌ Disadvantages

| # | Disadvantage | Description |
|---|--------------|-------------|
| 1 | **Over-Abstraction Risks** | Too much abstraction can make code harder to read and debug |
| 2 | **Initial Time Investment** | Writing reusable code often requires more upfront planning and effort |
| 3 | **Misuse** | Applying DRY inappropriately to unrelated functionalities can cause tight coupling and reduced flexibility |
| 4 | **Complex Refactoring** | Refactoring legacy code to adhere to DRY can be time-consuming and error-prone |

---

## 🎉 Conclusion

The **DRY principle** promotes readability, maintainability, and code efficiency — a fundamental guideline in software development. It lowers the chance of errors, improves collaboration, and facilitates maintenance by encouraging code reuse and decreasing redundancy.

The advantages of DRY must be weighed against potential drawbacks such as over-abstraction and premature optimisation. Used in conjunction with other complementary concepts (like SOLID), DRY encourages the development of **reliable, scalable, and maintainable** software systems.

---