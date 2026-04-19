# 🔒 Encapsulation in Java

---

## 📖 What is Encapsulation?

- Encapsulation is one of the **fundamental principles of Object-Oriented Programming (OOP)**
- It involves **bundling data (variables) and methods (functions)** that operate on the data into a **single unit called a class**
- Encapsulation also **restricts direct access** to certain components, ensuring **controlled interaction** through methods
- This **prevents unauthorized or accidental interference** with the object's data and ensures better control over the data flow in a program 🔐

---

## 🛠️ How is Encapsulation Achieved in Java?

In Java, encapsulation is typically achieved by:

- 🔴 **Declaring class variables as `private`** 🔐
- 🔵 **Providing public getter and setter methods** to access and modify these variables 🔑

---

## ⭐ Key Features of Encapsulation

### 🙈 1. Data Hiding
- **Prevents direct access to sensitive data**, ensuring that changes can only be made through **controlled methods**
- 📝 "Don't want you to touch" the raw data directly!

### 🧩 2. Modularity
- Promotes **modular design** by **separating data and behavior**, making the code easier to manage and debug
- → **Manageable code** ✅

### 🛡️ 3. Security *(V.V.V. Important!)*
- **Protects the integrity of the data** by restricting unwanted modifications
- Restricts access by **unwanted people/users** as well

### 🔀 4. Flexibility
- Allows developers to **change the internal implementation of a class** without affecting external code
- Setters add the **foundation of what you want to be SET** (with conditions/validation)

---

## 💻 Full Example — BankAccount (Encapsulation in Action)

```java
class BankAccount {
    // Private variables (data hiding) — Can't be accessed directly!
    private String accountNumber;
    private double balance;

    // Constructor
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }

    // Public getter method
    public String getAccountNumber() {
        return accountNumber;
    }

    // Public getter for balance
    public double getBalance() {
        return balance;
    }

    // Public setter method for deposit — "deposit" but based on a Condition
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: " + amount);
        } else {
            System.out.println("Invalid deposit amount.");
        }
    }

    // Public setter method for withdrawal — "withdraw" based on a Condition
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawn: " + amount);
        } else {
            System.out.println("Invalid withdrawal amount.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("12345", 1000.00);

        System.out.println("Account Number: " + account.getAccountNumber());
        System.out.println("Initial Balance: " + account.getBalance());

        account.deposit(500.00);  // These are handled via same Rules (conditions)
        System.out.println("Updated Balance: " + account.getBalance());

        account.withdraw(200.00); // These are handled via same Rules (conditions)
        System.out.println("Final Balance: " + account.getBalance());
    }
}
```

---

## 🔍 Breaking Down the Example

### 🔐 1. Private Variables
- The variables `accountNumber` and `balance` are **private**, **restricting direct access** from outside the class
- Nobody can do `account.balance = -9999` directly!

### 🌐 2. Public Methods
- **Getter methods** provide **controlled access** to the variables
- **Setter methods** ensure **valid updates** to the variables (with built-in condition checks)

### ✅ 3. Data Integrity
- The class ensures that **only valid operations** are performed on its data
- e.g. `deposit(-100)` → rejected, `withdraw(99999)` → rejected

---

## 🏆 Advantages of Encapsulation

### 🔒 1. Improved Data Security *(obviously 😄)*
- Prevents **unauthorized access and modifications** to the data
- Direct access to `balance` and `accountNumber` is **not allowed**, ensuring that sensitive information is **not exposed or modified arbitrarily**
- **Enhances data security and prevents accidental errors**

---

### 🔧 2. Ease of Maintenance
- Encapsulated code is **easier to modify and debug** without affecting external components
- 📝 We only know the **setter implementation** — that's the way something could be changed
- The `updateBalance` method is the **single point of truth** for both deposits and withdrawals. Any changes (e.g. adding transaction limits or fees) can be made in **this method alone** without affecting other parts of the code

---

### 🔀 3. Increased Flexibility
- **Internal implementation details can be changed without impacting external code**
- 📝 → Will see this further in **Design Patterns** — a better way to handle different **Fee Strategies**

#### 💻 Example — Adding Transaction Fee Internally (External code stays the same)

```java
// Internal implementation can be updated (e.g., adding transaction fee)
public void withdraw(double amount) {
    double transactionFee = 2.00; // New feature added internally
    if (amount > 0 && (amount + transactionFee) <= balance) {
        balance -= (amount + transactionFee);
        System.out.println("Withdrawn: " + amount + ", Fee: " + transactionFee);
    } else {
        System.out.println("Invalid withdrawal amount.");
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000.00);

        // External code remains unchanged despite internal changes
        account.withdraw(200.00);
        System.out.println("Remaining Balance: " + account.getBalance());
    }
}
```

> The **internal implementation of the withdraw method was updated** to include a transaction fee, but external code (e.g., the Main class) **didn't need any modifications**. This ensures **flexibility and backward compatibility**.

---

### 👁️ 4. Enhanced Readability
- Clearly defined interfaces improve **code readability and usability**
- At first glance → can see what all can happen! 👀
- The clear interface (`getAccountNumber`, `deposit`, and `withdraw`) makes it easy for developers to understand how to use the `BankAccount` class, **even if they are unfamiliar with its internal workings**

---

## ⚠️ Disadvantages of Encapsulation

### 📝 1. Slight Overhead *(Bakwas 😅)*
- → But **SAFE** (wearing protection can be overhead but veryyy useful 😉)
- Encapsulation introduces **additional boilerplate code** (getters and setters), which can increase verbosity
- Although encapsulation provides benefits, it can lead to **more verbose code**, especially when getters and setters are used extensively without adding significant value

#### 💻 Example — Boilerplate Overhead

```java
class BankAccount {
    private String accountNumber;
    private double balance;

    // Boilerplate getters and setters
    public String getAccountNumber() { return accountNumber; }
    public void setAccountNumber(String accountNumber) { this.accountNumber = accountNumber; }
    public double getBalance() { return balance; }
    public void setBalance(double balance) { this.balance = balance; }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        // Verbose code for simple tasks
        account.setAccountNumber("12345");
        account.setBalance(1000.00);
        System.out.println("Account Number: " + account.getAccountNumber());
        System.out.println("Balance: " + account.getBalance());
    }
}
```

---

### 🧩 2. Complexity
- **Beginners** may find it challenging to understand and implement encapsulation effectively
- For beginners, understanding why **direct access to fields is restricted** and why **getters/setters are used** can be confusing — this might make it harder for them to implement encapsulation correctly

#### 💻 Example — Complexity for Beginners

```java
class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) { this.balance = initialBalance; }
    public double getBalance() { return balance; }

    public void deposit(double amount) {
        if (amount > 0) balance += amount;
    }
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) balance -= amount;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000.00);
        // Beginners may find concepts like access modifiers and getters challenging
        account.deposit(500.00);
        account.withdraw(200.00);
        System.out.println("Final Balance: " + account.getBalance());
    }
}
```

---

## ✅ Conclusion

- **Encapsulation** is a vital aspect of OOP that fosters **secure, modular, and maintainable** code
- By controlling data access through encapsulation, developers can ensure **data integrity** and create **flexible, robust applications**
- Understanding and applying encapsulation effectively is essential for **building scalable Java programs**

---

## 📊 Quick Summary Table

| Feature | Description |
|---|---|
| **What** | Bundling data + methods into a class, restricting direct access |
| **How** | `private` fields + `public` getters/setters |
| **Data Hiding** | `private` prevents outside access |
| **Validation** | Setters enforce conditions before changing data |
| **Flexibility** | Internal changes don't break external code |
| **Readability** | Clear public interface for class usage |

---

> 💡 **Interview Tip:** Encapsulation = **`private` data + `public` controlled access**. The setter is not just a pass-through — it's a **gatekeeper** that validates input before modifying state. Always add validation in setters for production-quality code!