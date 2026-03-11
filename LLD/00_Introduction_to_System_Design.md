# Low-Level Design (LLD) 🏗️

## 📝 Introduction to LLD

**Low-Level Design (LLD)** is a detailed design phase in software development where the high-level architecture is broken down into granular components. It focuses on the **implementation details** of the system, including:

- Class diagrams
- Object interactions
- Method signatures
- Data structures used
- Algorithms for specific functionalities
- Database schema details

LLD is the blueprint that developers directly use to write code. It translates the **what** (from HLD) into **how** the system will be implemented.

---

## 🔧 How LLD Works

### 1️⃣ **Start with Requirements**
- Understand functional and non-functional requirements
- Identify key use cases and scenarios

### 2️⃣ **Design Classes and Objects**
- Define classes, their attributes, and methods
- Determine relationships (inheritance, composition, aggregation)
- Apply SOLID principles

### 3️⃣ **Create UML Diagrams**
- **Class Diagrams**: Show classes and their relationships
- **Sequence Diagrams**: Show object interactions over time
- **Activity Diagrams**: Show workflow and logic flow

### 4️⃣ **Define Data Models**
- Design database schemas
- Define tables, columns, relationships, and constraints
- Plan indexing strategies

### 5️⃣ **Algorithm Design**
- Choose appropriate algorithms for operations
- Consider time and space complexity
- Optimize for performance

### 6️⃣ **Handle Edge Cases**
- Plan for error handling
- Validate inputs
- Consider concurrency and thread safety

---

## 🎯 LLD vs HLD vs DSA

| Aspect | **LLD (Low-Level Design)** | **HLD (High-Level Design)** | **DSA (Data Structures & Algorithms)** |
|--------|---------------------------|---------------------------|--------------------------------------|
| **Focus** | Implementation details | System architecture | Problem-solving techniques |
| **Level** | Component/Class level | System level | Algorithmic level |
| **Output** | Class diagrams, sequence diagrams, code-ready design | Architecture diagrams, component diagrams, system flow | Optimal solutions, code snippets |
| **Audience** | Developers | Architects, stakeholders | Developers, competitive programmers |
| **Goal** | How to implement features | What components are needed | How to solve specific problems efficiently |
| **Scope** | Single feature/module | Entire system | Individual problems |
| **Detail Level** | Very detailed (methods, attributes) | Abstract (modules, services) | Focused on logic and efficiency |

---

## 🆚 Detailed Comparison

### 🔵 **Low-Level Design (LLD)**

**What it is:**
- Detailed design of individual components
- Focuses on classes, methods, and their interactions
- Implements design patterns

**When to use:**
- When you need to implement a specific feature
- During coding phase
- For designing reusable components

**Example:**
```
Design a Parking Lot System
- Classes: ParkingLot, Floor, Slot, Vehicle, Ticket
- Methods: bookSlot(), releaseSlot(), calculateFee()
- Relationships: ParkingLot has multiple Floors, Floor has multiple Slots
```

**Key Activities:**
- ✅ Define class structures
- ✅ Write method signatures
- ✅ Design database schemas
- ✅ Plan object interactions
- ✅ Apply design patterns

---

### 🟢 **High-Level Design (HLD)**

**What it is:**
- System architecture and overall structure
- Focuses on modules, services, and communication
- Defines technology stack and infrastructure

**When to use:**
- At the beginning of a project
- When planning system scalability
- For discussing with stakeholders

**Example:**
```
Design an E-commerce System
- Components: User Service, Product Service, Order Service, Payment Gateway
- Communication: REST APIs, Message Queues
- Storage: SQL Database, Redis Cache, S3 for images
- Architecture: Microservices, Load Balancer, CDN
```

**Key Activities:**
- ✅ Identify system components
- ✅ Define APIs and protocols
- ✅ Choose databases and caching strategies
- ✅ Plan for scalability and availability
- ✅ Design deployment architecture

---

### 🟡 **Data Structures & Algorithms (DSA)**

**What it is:**
- Fundamental problem-solving techniques
- Focuses on efficient data organization and manipulation
- Core building blocks for coding

**When to use:**
- When solving algorithmic problems
- For optimizing code performance
- In competitive programming and interviews

**Example:**
```
Problem: Find the shortest path in a graph
- Data Structure: Graph (Adjacency List)
- Algorithm: Dijkstra's Algorithm or BFS
- Complexity: O(V + E log V)
```

**Key Activities:**
- ✅ Choose appropriate data structures
- ✅ Implement efficient algorithms
- ✅ Analyze time and space complexity
- ✅ Optimize solutions
- ✅ Handle edge cases

---

## 🔄 How They Work Together

```
HLD (What & Architecture)
        ↓
LLD (How & Implementation)
        ↓
DSA (Efficient Solutions)
        ↓
    Final Code
```

### Example: Design a URL Shortener

**HLD:**
- API Gateway
- Application Servers
- Database (SQL/NoSQL)
- Cache (Redis)
- Load Balancer

**LLD:**
- Class: URLShortener
    - Methods: generateShortURL(), getLongURL()
- Class: Database
    - Tables: urls(id, short_url, long_url, created_at)
- Design pattern: Factory pattern for URL generation

**DSA:**
- Hash function for generating unique short URLs
- Base62 encoding
- Trie or HashMap for fast lookups
- Time Complexity: O(1) for retrieval

---

## 🎓 When to Use What?

### Use **HLD** when:
- 📋 Planning a new system
- 🔧 Choosing technologies
- 📊 Presenting to stakeholders
- 🌐 Designing for scalability

### Use **LLD** when:
- 💻 Ready to write code
- 🏗️ Implementing a feature
- 🔨 Refactoring existing code
- 📚 Creating reusable components

### Use **DSA** when:
- ⚡ Optimizing performance
- 🧩 Solving algorithmic problems
- 🏆 Preparing for interviews
- 🔍 Implementing core logic

---

## 📚 LLD Best Practices

### 1️⃣ **Follow SOLID Principles**
- **S**ingle Responsibility Principle
- **O**pen/Closed Principle
- **L**iskov Substitution Principle
- **I**nterface Segregation Principle
- **D**ependency Inversion Principle

### 2️⃣ **Use Design Patterns**
- Creational: Singleton, Factory, Builder
- Structural: Adapter, Decorator, Facade
- Behavioral: Strategy, Observer, Command

### 3️⃣ **Plan for Extensibility**
- Design for future changes
- Avoid tight coupling
- Use interfaces and abstractions

### 4️⃣ **Document Clearly**
- Add comments for complex logic
- Create class and sequence diagrams
- Write clear method descriptions

### 5️⃣ **Consider Performance**
- Choose efficient data structures
- Optimize database queries
- Plan for caching

---

## 💡 Key Takeaways

| Concept | Summary |
|---------|---------|
| **LLD** | Detailed implementation design - classes, methods, databases |
| **HLD** | System architecture - components, services, infrastructure |
| **DSA** | Problem-solving - algorithms, data structures, optimization |
| **Relationship** | HLD → LLD → DSA → Code |
| **Focus** | LLD bridges the gap between design and code |
