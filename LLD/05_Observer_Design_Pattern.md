# 👁️ Observer Design Pattern

---

## 📖 Definition

> **The Observer pattern** is a **behavioral design pattern** that defines a **one-to-many dependency** between objects. When one object (the **subject**) changes state, all its **dependents (observers)** are **automatically notified and updated**.

---

## 🔴 PROBLEM STATEMENT

You have an object whose state changes over time, and **multiple other objects** need to know about these changes. The challenges are:

### 1️⃣ Tight Coupling
**Problem:** You don't want the subject to know the concrete details of all objects that need updates.

### 2️⃣ Dynamic Subscriptions
**Problem:** Objects should be able to **subscribe / unsubscribe** from updates at runtime.

### 3️⃣ Broadcast Communication
**Problem:** One change should **notify multiple interested parties automatically**.

### 4️⃣ Maintainability
**Problem:** Adding new observers shouldn't require **modifying the subject's code**.

---

## 🎯 Observer Pattern Solution

### 🏗️ Architecture Diagram

```
┌─────────────────────────────────┐              ┌──────────────────────┐
│   Observable Interface          │              │  Observer Interface  │
├─────────────────────────────────┤              ├──────────────────────┤
│ List<ObserverIntf> list;        │              │                      │
│                                 │   (0..*)     │  + update()          │
│ + add(ObserverIntf obj);        │─────────────>│                      │
│ + remove(ObserverIntf obj);     │   has-a      │                      │
│ + notify();                     │              │                      │
│ + setData();                    │              │                      │
│                                 │              │                      │
└─────────────────────────────────┘              └──────────────────────┘
                △                                           △
                │                                            │
                │ is-a                                       │ is-a
                │                                            │
┌───────────────────────────────┐              ┌──────────────────────┐
│ Observable Concrete Class     │              │ Observer Concrete    │
├───────────────────────────────┤              │ Class(es)            │
│ add(ObserverIntf obj) {       │              ├──────────────────────┤
│   list.add(obj);              │              │ + update()           │
│ }                             │              │                      │
│                               │              │                      │
└───────────────────────────────┘              └──────────────────────┘
```
