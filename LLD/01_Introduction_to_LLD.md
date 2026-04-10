# 🏗️ Low Level Design (LLD)

## 📌 What is LLD?

- 🔍 **Low-Level Design (LLD)** is a detailed phase in the software development process that **focuses** on **designing the individual components** outlined in the **High-Level Design (HLD)**.
  > 💡 HLD → **CODE**

- ⚙️ LLD delves into the specifics, defining how **modules**, **classes**, **functions**, and **data structures** interact to achieve the desired functionality.

---

## 🎯 Example

> 🐧 **For example**, in a **movie ticket booking system**, LLD would detail how components like:
> - 🪑 **Seat Selection**
> - 💳 **Payment Processing**
> - 🎟️ **Ticket Generation**
>
> ...are implemented and interact with one another.

---

> 🏢 *Used by companies ranging from large enterprises like **Microsoft** to startups like **Refyne**.*

---

## ❓ How is LLD Different from HLD?

> 🤔 *(Biggest Doubt for most beginners!)*

**Low-Level Design (LLD)** and **High-Level Design (HLD)** are two key stages in software development, focusing on different levels of detail.

| Stage | Type | Focus |
|-------|------|-------|
| 1️⃣ **HLD** — High-Level Design | *1st Stage* | Overall system architecture & components |
| 2️⃣ **LLD** — Low-Level Design  | *2nd Stage* | Detailed design of individual components |

---

### 🏛️ High-Level Design (HLD)

> 🧠 **High-Level Design (HLD)** focuses on the **overall architecture** of the system.

**HLD flow (example):**
```
API Gateway  →  Load Balancing  →  Services  →  Types of DBs / Cache
                                                        ↓
                                              Consistency / Availability
```

For instance, in a **movie ticket booking system**, HLD would outline the main components like:

- 🖥️ **User Interface** — where users select movies and seats
- ⚙️ **Backend Services** — handling booking requests, seat availability, and notifications
- 🗄️ **Database** — storing movie schedules, user data, and bookings

> 🔄 It would also define how these components interact — like the **flow of data** between the **user interface**, **backend**, and **third-party payment gateways**.

---

### 🔩 Low-Level Design (LLD)

> 🧠 **Low-Level Design (LLD)**, on the other hand, dives into the specifics of **implementing individual features**.

For example, it defines how the **booking process works** — detailing the **step-by-step flow** from when a user selects a movie and showtime to when a ticket is successfully booked.

> 📝 *Main focus → Mostly **Services***

**Booking flow:**
```
 ✅ Select Movie  →  ✅ Choose Seat  →  ✅ Make Payment  →  ⏳ Confirm Booking
```

```
🎬 MOVIE TIME   →→→   [ BOOKED ✅ ]   →→→   🎉 Enjoy!
```

It specifies how:

- ✔️ **Data is validated** — e.g., ensuring selected seats are available and payment details are correct
- 🔒 **Algorithms for locking seats** — to prevent double booking *(→ Array works internally)*
- 🗃️ **Booking information is stored** in the database *(schema)*

> ⚠️ *Note: Don't go deep into Data Modelling at this stage — that's covered separately.*

LLD also describes the **flow of data**, such as how a **booking confirmation** is generated and sent to the user via **email or SMS**. It's like creating **blueprints** for each transaction in the system, covering the smallest details to **ensure reliability and precision**. 🛡️✅

---

## 💡 Key Takeaway — HLD vs LLD

> 🚨 While **HLD sets the vision for the system**, **LLD ensures every feature** — like booking tickets — is implemented accurately and aligns with the high-level plan.

| | 🏛️ HLD | 🔩 LLD |
|---|---|---|
| **Role** | Sets the **vision** | Ensures **implementation** |
| **Focus** | Big picture / Architecture | Individual features & logic |
| **Example** | Define components (UI, DB, Services) | How seat locking algorithm works |

---

## 🧱 Building Blocks of LLD

```
+--------+  +--------+  +--------+  +--------+
| Block1 |  | Block2 |  | Block3 |  | Block4 |
|   🟩   |  |   🟨    |  |   🟧   |  |   🟥    |
+--------+  +--------+  +--------+  +--------+
```

---

### 1️⃣ 📋 Requirement Gathering

> 💡 *(Understand the Problem → **Activity Diagram**)*

Understand the **detailed requirements** of the system. This includes identifying the **functionality, constraints, and edge cases** to ensure the design meets user needs.

For example, in a **movie ticket booking system**, this would include functionality such as:

- 🪑 **Seat Selection**
- 💳 **Payment Processing**
- 🎟️ **Ticket Generation**
- ⚠️ **Simultaneous bookings for the same seat**

---

### 2️⃣ 📐 Laying Down Use Cases

> 💡 *(Find out **Main Entities**) + (Make the Problem **Concise**)* ✅

Define specific scenarios that the system will handle, **outlining inputs, actions, and expected outputs**. Use cases help in **clarifying the scope** and **guiding the design process**.

For a **movie ticket booking system**, use cases would include:

- 🎫 Booking tickets for a **single user**
- 👥 Handling **group bookings** with seat proximity
- ❌ **Canceling** a booking and **refunding** payment
- 🔔 Sending **notifications (SMS/email)** for booking confirmation

---

# 📊 UML Diagrams
> 💡 *Just know about them* → Not much time in Interviews → **Practical advice**

Create diagrams to visually represent the structure, behavior, and interactions of system components.

In a **movie ticket booking system**, these could include:

- 🔵 **Class Diagram:** Representing entities like `User`, `Ticket`, `Movie`, `Payment`, and `Theater`.
- 🔵 **Sequence Diagram:** Showing the flow of actions for booking a ticket, including seat selection, payment processing, and confirmation.
- 🔵 **Activity Diagram:** Mapping the workflow for booking and canceling tickets.

---

> 🐧 Below is a **use case diagram** representing the key interactions within a movie ticket booking system, which we will reference to understand how various actors and use cases collaborate to fulfill the system's objectives.

---

### 4️⃣ 🧩 Model Problems

> 💡 *(Solve specific problems → **Design Patterns**)*

This phase focuses on **solving specific problems identified during the use case analysis**. It involves breaking down the system into smaller components and addressing individual design challenges.

Design patterns play a crucial role in this phase, offering **reusable solutions** to common design problems. Some of the most famous Design Patterns include:

- 🏭 **Factory Design Pattern**
- 🎯 **Strategy Design Pattern**
- 👁️ **Observer Design Pattern**
- 🔂 **Singleton Design Pattern**
- ...and more!

```
Use Case Analysis  →  Identify Problems  →  Apply Design Patterns  →  Solve! ✅
```

---

### 5️⃣ 💻 Implement Code

> ⚠️ *(Hardest part)* → ⏱️ **in ~45 min**

```
       ↙  ↓  ↘
   ( hardest ) → in 45 min
```

Translate the design into **clean, modular, and efficient code**, adhering to coding standards and principles such as:

- 🧱 **SOLID** — for robust object-oriented design
- 🔁 **DRY** *(Don't Repeat Yourself)* — to ensure maintainability and scalability

> 🎯 *Goal: Write code that is readable, reusable, and reliable.*

---

## 🚀 How to Approach LLD Problems?

> ✨ *Cool!* ✓

🧠 **Low-Level Design (LLD)** problems often form a **critical component of technical interviews**, especially for **backend engineering roles**. They evaluate your ability to design software systems at a detailed level, focusing on:

→ 1. 🏛️ **Class structures**
→ 2. 🎨 **Design patterns**
→ 3. 🔗 **Relationships**
→ 4. ⚙️ **Implementation details**

---

> 🐧 Here's a **structured approach** to tackle LLD problems effectively, using a **movie ticket booking system** as an example:

---

### 1️⃣ 🔍 Clarify Requirements and Use Cases

> ⭐ *Most Important Step!*

The interview begins with a **brief introduction**, where the interviewer explains the format and provides a **high-level overview** of the system you'll design.
Afterward, the **problem statement** is presented.

*Here's how the conversation might unfold:*

> 🧑‍💼 **Interviewer:** Design a **Movie ticket booking system**,

> 🙋 **Interviewee:** Absolutely! Based on the problem, here's my understanding of the system so far:
>
> - Users will **browse theaters** *(then screens)*, **select movies** or shows, **book seats**, and **make payments.**
> - The system needs to handle **concurrent bookings** efficiently.
>
> Would you like me to add any details to this understanding?

> 💡 *Activities done by user → Mimic the user behavior*

> 🧑‍💼 **Interviewer:** That's a good summary. We can move ahead

> 🙋 **Interviewee:** To clarify further:
> → Should we support **multiple user types**, like admins and customers?
> → How do we handle **seat availability** during concurrent bookings?
> → Is **payment processing** and **ticket generation** included in scope?

> 🧑‍💼 **Interviewer:** Yes, **concurrency and payment are key**. Admin roles and advanced features are out of scope for now.

> ⭐ *Most Important → Get out what **Interviewers want** 😉*

> 🙋 **Interviewee:** Understood. Here's the **core flow** I'll focus on:
>
> - 🔎 Users **search for movies** based on location.
> - 🗺️ Movies are **mapped to screens** and screens to theaters.
> - 🪑 Seats are **selected and locked** for booking.
> - 💳 **Payment processing** confirms the booking.
>
> Does this cover the main use cases?

> 🧑‍💼 **Interviewer:** Yes, that looks great. **Proceed ahead.**

---

### 2️⃣ 🧩 Identify Entities (Classes)

Once the requirements and the use-cases are clear, **break the system down into core entities**.

> 🙋 **Interviewee:** Based on the requirements, I've identified these **core entities**:
>
> - 👤 **User** ✓
> - 🎬 **Movie**
> - 🏛️ **Theater**
> - 🎞️ **Show**
> - 🪑 **Seat**
> - 📋 **Booking**
> - 💳 **Payment**

---

> ⏱️ **MOST IMPORTANT** *(Time Management)*
>
> 💡 **NOTE:** Avoid excessive entity details ~~as they~~ consume time better spent **explaining** the **core problem's solution**.

---

### 3️⃣ 🔗 Define Relationships Between Entities

> 💡 *→ with the help of **User Activities***

> Once the entities are clear, proceed with **mapping their relationships and interdependencies**.

> 🙋 **Interviewee:** Here's how the entities are related:
>
> - → 🏛️ **A Theater has multiple Screens, each linked to specific Seats.**
> - → 🎞️ **A Show occurs on a specific Screen and maps to a Movie.**
> - → 🪑 **A Seat is tied to a Show for tracking availability during bookings.**
> - → 📋 **A Booking associates a User with Seats, Show, and Payment.**

---

> 🐧
> - A Show has a **`List<Seat>`** to track seat availability during that specific screening.
> - A Show is also **uniquely associated** with a specific **movie** and **screen**. Below is the example implementation of a show class.

```java
public class Show {
    private final String id;
    private final Movie movie;
    private final Screen screen;
    private final Date startTime;
    private final Integer durationInMinutes;
}
```

> ⚠️ **Attention:** No class diagram is made — *no time in Interviews!*
>
> 💡 *Ask Interviewer (Mostly ignore)*

---

> - 🏛️ A **Theater** contains multiple **Screens.**
> - 🖥️ Each **Screen** is responsible for managing its **Seats.**

```java
public class Theater {
    private final String id;
    private final String name;
    private final String location;
    private final List<Screen> screens;
}

public class Screen {
    private final String id;
    private final String name;
    private final List<Seat> seats;
}
```

> 🪑 **Seat** → `Screen` ? or `Show` ? — *do anything!*
>
> ✅ Just **focus on the CORE PROBLEM.**
>
> 📦 **Mostly, the Interviewer will not run the code.**

```java
public class Booking {
    private final String id;
    private final User user;
    private final Show show;
    private final List<Seat> bookedSeats;
    private final Payment payment;
}

public class Payment {  // 💳 Payment Method
    private final String id;
    private final double amount;
    private final PaymentStatus status;
    private final Date timestamp;
}
```

> 🙋 **Interviewee:** Does this relationship structure align with the requirements?

> 🧑‍💼 **Interviewer:** Yes, this is aligned. Please proceed ahead.

---

### 4️⃣ 🛠️ Identify Core Methods for Each Class

Once the entities and properties are defined, the next step is to focus on the **methods/operations** that drive the system's functionality, and when they should be built to support a **clean code architecture**.

> These methods should address primary use cases like **booking seats**, **managing availability**, and **ensuring concurrency** to meet user requirements.

> 🙋 **Interviewee:** Moving on to the primary functionalities, I'll focus on the **major methods** which are critical to the system.

---

#### 🪑 Seat Management

> - → `lockSeats(List<Seat> seats, int showId)` — Locks selected seats for a user, **preventing others from booking them** during the session.
> - → `releaseSeats(List<Seat> seats, int showId)` — Releases previously locked seats if the booking is **canceled or not confirmed**.

---

#### 📋 Booking Management

> - → `createBooking(int userId, int showId, List<Integer> seatIds)` — Creates a **new booking** for the user with the selected seats.
> - → `cancelBooking(int bookingId)` — **Cancels** a booking and releases the locked seats.

---

#### 🔒 Concurrency Handling *(while booking seats — locks)*

> - To manage concurrency, we can use **synchronized blocks** or **distributed locks** to ensure that multiple users **cannot book the same seats** at the same time.
> - Additionally, implementing a **retry mechanism** *(e.g., exponential backoff)* in case of seat locking failures can further improve the **robustness** of the system.

> 💡 *Just tell — don't code it.*

---

#### 🎨 Design Patterns *(very important)*

> - → **Strategy Pattern** for handling **different payment methods** — allows the system to switch between payment methods *(credit card, wallet, etc.)* **without modifying** the booking logic.
> - → **Factory Pattern** to create **different types of bookings** *(VIP, Regular, etc.)* based on user preferences or seat categories — centralizes object creation and makes the system **easier to scale** with minimal changes.

> 💡 *Can just tell !!* — No need to implement in the interview.

---

> 🙋 **Interviewee:** Would you like me to dive deeper into any particular functionality?

> 💡 ***Ask !!!*** ← *(Always ask the interviewer to guide the focus)*

> 🧑‍💼 **Interviewer:** Let's focus on the `lockSeats` and `createBooking` methods.

---

> ⭐ **Note: Focus on implementing core business logic rather than basic CRUD operations.**
>
> The interviewer primarily evaluates your approach to **designing and implementing complex system functionalities**.

---

### 5️⃣ 💻 Implement Necessary Methods

> 🙋 **Interviewee:** I'll begin with the `lockSeats` method. Would you prefer I explain the approach first or code while explaining?

> 🧑‍💼 **Interviewer:** You may code and explain simultaneously.

> 🙋 **Interviewee:** I'll proceed with the implementation.

> *While coding, maintain a clear structure and use design principles like **SOLID, DRY, KISS, YAGNI**, etc. Explain your choices to the interviewer — like what design patterns you can use and why they are used, validate your approach, and ensure alignment with the overall architecture.*

```java
public boolean lockSeats(List<Integer> seatIds, int showId, int userId) {
    synchronized (this) {
        for (int seatId : seatIds) {
            Seat seat = seatRepository.getSeat(seatId, showId);
            if (seat.getStatus() != SeatStatus.AVAILABLE) {
                return false; // Seat already locked or booked
            }
            seat.setStatus(SeatStatus.LOCKED);
            seat.setLockedBy(userId); // Associate the locked seat with the user ID
            seatRepository.updateSeat(seat);
        }
    }
    return true;
}
```

> *Once the interviewer approves your approach, proceed with implementing the next method.*

---

> 🧑‍💼 **Interviewer:** We can proceed with the `createBooking()` method implementation now.

> 🙋 **Interviewee:** Yes sure.

```java
public Booking createBooking(final String userId, final Show show, final List<Seat> seats) {
    // Check if any seat is already booked
    if (isAnySeatAlreadyBooked(show, seats)) {
        throw new SeatPermanentlyUnavailableException();
    }
    // Attempt to lock the seats for the user
    boolean lockSuccess = seatLockProvider.lockSeats(seats, show.getId(), userId);
    if (!lockSuccess) {
        throw new SeatLockFailedException("Failed to lock seats for the user");
    }
    // Generate a unique booking ID
    final String bookingId = UUID.randomUUID().toString();
    final Booking newBooking = new Booking(bookingId, show, userId, seats);
    // Store the booking
    showBookings.put(bookingId, newBooking);
    // Return the new booking
    return newBooking;
}
```

---

### 6️⃣ ⚠️ Exception Handling

Incorporate error handling to manage edge cases gracefully. Proper error handling ensures the system **behaves predictably** even under unexpected conditions, enhancing the **user experience** and **system reliability**.

> **Examples:**
>
> - ❌ **No Seats Available** — Ensure that users are informed when attempting to book seats that are **already reserved**.

```java
private boolean isAnySeatAlreadyBooked(final Show show, final List<Seat> seats) {
    final List<Seat> bookedSeats = getBookedSeats(show);
    for (Seat seat : seats) {
        if (bookedSeats.contains(seat)) {
            return true;
        }
    }
    return false;
}

public Booking createBooking(final String userId, final Show show, final List<Seat> seats) {
    if (isAnySeatAlreadyBooked(show, seats))
        throw new SeatPermanentlyUnavailableException();
    // rest of the logic
}
```

> - 🚫 **Payment Failure** — Handle failed transactions and provide options for **retrying** or **canceling** the booking process.

```java
public void processPaymentFailed(final Booking booking, final String user) {
    if (!booking.getUser().equals(user)) {
        throw new BadRequestException();
    }
    if (!bookingFailures.containsKey(booking)) {
        bookingFailures.put(booking, 0);
    }
    final Integer currentFailuresCount = bookingFailures.get(booking);
    final Integer newFailuresCount = currentFailuresCount + 1;
    bookingFailures.put(booking, newFailuresCount);
    if (newFailuresCount > allowedRetries) {
        seatLockProvider.unlockSeats(booking.getShow(), booking.getSeatsBooked(), booking.getUser());
    }
}
```

---

## 🗂️ Class Diagram — Movie Ticket Booking System

```
┌─────────────────────────────────────┐        ┌──────────────────────────────┐
│ © BookingService                    │        │ © Booking                    │
│─────────────────────────────────────│        │──────────────────────────────│
│ -Map<String, Booking> showBookings  │        │ -String id                   │
│ -SeatLockProvider seatLockProvider  │        │ -User user                   │
│─────────────────────────────────────│        │ -Show show                   │
│ +createBooking(userId, show, seats) │        │ -List<Seat> bookedSeats      │
│ +isAnySeatAlreadyBooked(show, seats)│        │ -Payment payment             │
│ +processPaymentFailed(booking, user)│        └──────────────────────────────┘
│  📝 Handles seat locking &          │
│     booking creation                │         ┌──────────────────────────────┐
└─────────────────────────────────────┘         │ © Theater                    │
           uses ↓          uses ↓               │──────────────────────────────│
┌───────────────────────┐  ┌─────────────────┐  │ -String id                   │
│ © SeatLockProvider    │  │ © SeatRepository│  │ -String name                 │
│───────────────────────│  │─────────────────│  │ -String location             │
│ +lockSeats(seats,     │  │ +getSeat(       │  │ -List<Screen> screens        │
│   showId, userId)     │  │   seatId,showId)│  └──────────────────────────────┘
│ +unlockSeats(show,    │  │ +updateSeat(    │
│   seats, userId)      │  │   seat)         │   ┌──────────────────────────────┐
│ 📝 Manages concurrent │  └─────────────────┘   │ © Screen                     │
│    seat operations    │                        │──────────────────────────────│
└───────────────────────┘                        │ -String id                   │
                                                 │ -String name                 │
┌──────────────────────────────┐                 │ -List<Seat> seats            │
│ © Show                       │                 └──────────────────────────────┘
│──────────────────────────────│
│ -String id                   │  ┌──────────────────────────────┐
│ -Movie movie                 │  │ © Payment                    │
│ -Screen screen               │  │──────────────────────────────│
│ -Date startTime              │  │ -String id                   │
│ -Integer durationInMinutes   │  │ -double amount               │
└──────────────────────────────┘  │ -PaymentStatus status        │
                                  │ -Date timestamp              │
┌──────────────────────────────┐  └──────────────────────────────┘
│ © Seat                       │
│──────────────────────────────│  ┌──────────────────────────────┐
│ -String id                   │  │ Ε PaymentStatus (enum)       │
│ -SeatStatus status           │  │──────────────────────────────│
│ -String lockedBy             │  │  PENDING                     │
│──────────────────────────────│  │  COMPLETED                   │
│ +setStatus(SeatStatus)       │  │  FAILED                      │
│ +setLockedBy(userId)         │  └──────────────────────────────┘
└──────────────────────────────┘
         has ↓
┌──────────────────────────────┐
│ Ε SeatStatus (enum)          │
│──────────────────────────────│
│  AVAILABLE                   │
│  LOCKED                      │
│  BOOKED                      │
└──────────────────────────────┘
```

---

## 🏁 Conclusion

The art of Low-Level Design interviews lies not just in **technical knowledge**, but in demonstrating a **methodical approach** to problem-solving. Success comes from:

### 1. 📈 Systematic Progression

> - Begin with **thorough requirement gathering**
> - Move systematically from **high-level entities** to **detailed implementations**
> - Validate your approach at each step with the interviewer ✅

### 2. 🧠 Practical Considerations

> - Focus on **maintainable and scalable** solutions
> - Address critical aspects like **concurrency** and **error handling**
> - Demonstrate knowledge of **design patterns** where relevant 🧩

### 3. 🗣️ Communication Excellence

> - Clearly **articulate your design choices**
> - Explain **tradeoffs** in your decisions 💬
> - Show **adaptability** when receiving interviewer feedback 🌱

---

> 📌 **Remember:**
>
> A successful LLD interview is not about creating a **perfect design** in the first attempt. Instead, it's about demonstrating:
>
> - Your **structured thinking process**
> - Your ability to **translate requirements into concrete solutions**
> - Your understanding of **object-oriented design principles** 💡
> - Your skills in writing **clean, maintainable code** 🧹
> - Your **openness to feedback** and ability to **iterate on your design** 🔄

By following this systematic approach and maintaining **clear communication** throughout the interview, you'll be well-equipped to tackle any **Low-Level Design challenge** effectively. 💪🎯
