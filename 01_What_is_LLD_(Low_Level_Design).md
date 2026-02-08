- ### It majorly focuses on **classes** and **objects** withing a system.

![Representation of LLD](IMAGES/LEC_1/IMG_1.jpg)

- ### Goal is :
  - To write <u>*clean code*</u>.
  - Code should be <u>*flexible*</u> and <u>*maintainable*</u>.
  - Code should be <u>*easy to test*</u>.

![Categories of Design Pattern](IMAGES/LEC_1/IMG_2.jpg)

- ### CREATIONAL
  - It controls the object creation.
  - Various type of ***Creational Patterns*** are :
    - *Singleton*
    - *Builder*
    - *Factory*
    - *Abstract Factory*
    - *Object Pool*
    - *Prototype*

---

- ### STRUCTURAL
  - It focuses on, how different ***classes/objects*** are arranged together so that **larger problem** can be solved in **most flexible way**.
  - Various patterns are : 
    - *Decorator*
    - *Proxy*
    - *Composite*
    - *Adapter*
    - *Bridge*
    - *Façade*
    - *Flyweight*

---

- ### BEHAVIOURAL
  - It focuses on, how different ***objects communicate or interact*** with each other.
  - Like in other words, with above Structural Pattern we created how the skeleton behaves (**coordination, responsibility, interaction**) is all guided by Behavioral Pattern.
  - Various patterns are : 
    - *State* 
    - *Strategy*
    - *Observer*
    - *Chain of Responsibility*
    - *Template*
    - *Iterator*
    - *Interpreter*
    - *Command*
    - *Visitor*
    - *Mediator*
    - *Memento*
    - *Null Object*

---

# ***has-a AND is-a RELATIONSHIP***
### ==========================================

- ### is-a
  - This is nothing but *inheritance*.

![Pictorial Representation of is-a Relationship](IMAGES/LEC_1/IMG_3.jpg)

- ### has-a
  - Shows a **link between 2 objects**.
    - *House has rooms*.
    - *Library has books*.
    - *School has students* etc.

![Types of Association](IMAGES/LEC_1/IMG_4.jpg)

---

- ### Association
  - General term for has-a relationship.
  - *Association Representation*

![Types of Association](IMAGES/LEC_1/IMG_5.jpg)

---

# TYPES OF ASSOCIATION
### ==========================================

- ### Weak Relationship (*Aggregation*)
  - ***Existence of one object is not dependent on another***.
  - *Ex* : *Library hs books*.

![Types of Association](IMAGES/LEC_1/IMG_6.jpg)

  - Here both are independent of each other. If *books are not present, Library can still exist* and similarly if *Library does not exist, books can still exist alone*.
  - ```java
    public class Library{
        List<Books> books;
    }
    
---

- ### Strong Relationship (*Composition*)]
  - ***Existence of one object is dependent on another***.
  - *Ex* : *House has rooms*.
  - Here **rooms existence depends on House**, *if House does not exist, room will also not exist*.

![Types of Association](IMAGES/LEC_1/IMG_7.jpg)

  - ```java
    public class House {
      List<Rooms> rooms;
      // also takes care of creation and managing of Room objects
      public House(){
        rooms = new ArrayList<>();
        rooms.add(new Room("Living Room"));
        rooms.add(new Room("Bedroom"));
      }
    }