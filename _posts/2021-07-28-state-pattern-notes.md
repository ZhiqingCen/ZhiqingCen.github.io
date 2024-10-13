---
title: State Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_the class behaviour changes based on its state_**

we create objects which represent various states and a context object whose behaviour varies as its state object changes

- Benefits
  - It keeps the `state-specific` behaviour
  - It makes any `state transitions` explicit
- Usage
  - When the behaviour of object depends on its `state` and it must be able to **change its behaviour at runtime** according to the new `state`
  - It is used when the operations have **large, multipart conditional statements** that depend on the `state` of an object

## Implementation

1. Create a `Connection` interface that will provide the connection to the Controller class

   ```java
   public interface Connection {
       public void open();
       public void close();
       public void log();
       public void update();
   }
   ```

2. Create an `Accounting`, `Sales`, `Management` classes that will implement to the `Connection` interface

   ```java
   public class Accounting implements Connection {
       @Override
       public void open() {
           System.out.println("open database for accounting");
       }

       @Override
       public void close() {
           System.out.println("close the database");
       }

       @Override
       public void log() {
           System.out.println("log activities");
       }

       @Override
       public void update() {
           System.out.println("Accounting has been updated");
       }
   }
   ```

   ```java
   public class Sales implements Connection {
       @Override
       public void open() {
           System.out.println("open database for sales");
       }

       @Override
       public void close() {
           System.out.println("close the database");
       }

       @Override
       public void log() {
           System.out.println("log activities");
       }

       @Override
       public void update() {
           System.out.println("Sales has been updated");
       }
   }
   ```

   ```java
   public class Management implements Connection {
       @Override
       public void open() {
           System.out.println("open database for Management");
       }

       @Override
       public void close() {
           System.out.println("close the database");
       }

       @Override
       public void log() {
           System.out.println("log activities");
       }

       @Override
       public void update() {
           System.out.println("Management has been updated");
       }
   }
   ```

3. Create a `Controller` class that will use the `Connection` interface for connecting with different types of connection

   ```java
   public class Controller {
       public static Accounting acct;
       public static Sales sales;
       public static Management management;

       private static Connection con;

       Controller() {
          acct = new Accounting();
          sales = new Sales();
          management = new Management();
       }

       public void setAccountingConnection() {
           con = acct;
       }
       public void setSalesConnection() {
           con  = sales;
       }
       public void setManagementConnection() {
           con  = management;
       }
       public void open() {
           con.open();
       }
       public void close() {
           con.close();
       }
       public void log() {
           con.log();
       }
       public void update() {
           con.update();
       }
   }
   ```

4. Create a `StatePatternDemo` class

   ```java
   public class StatePatternDemo {
       Controller controller;

       StatePatternDemo(String con) {
           controller = new Controller();
           //the following trigger should be made by the user
           if(con.equalsIgnoreCase("management"))
            controller.setManagementConnection();
           if(con.equalsIgnoreCase("sales"))
            controller.setSalesConnection();
           if(con.equalsIgnoreCase("accounting"))
                controller.setAccountingConnection();
           controller.open();
           controller.log();
           controller.close();
           controller.update();
       }

       public static void main(String args[]) {
           // input: java StatePatternDemo Sales
           new StatePatternDemo(args[0]);
       }
   }
   ```

## Notes

- A `state` is a description of the status of a system that is waiting to execute a transition
- A `transition` is a set of actions to be executed when a condition is fulfilled or when an event is received
- **_Identical stimuli trigger different actions depending on the current state_**
- Often, the following are also associated with a state:
  - an **entry action**: performed when entering the state
  - an **exit action**: performed when exiting the state

---

### Automata Theory

- Combinational logic <= Finite-state machine <= Pushdown automaton <= Turing Machine
- `Finite-state Machine` (FSM) / finite-state automaton / finite automaton / state machine
  - an abstract machine that can be in exactly one of a finite number of states at any given time.
    - transition: can change from one state to another in response to some external inputs.
  - An finite-state machine is defined by
    - a list of its states
    - the conditions for each transition
    - its initial state

---

### State Transition Table Example

| Current State | Input | Next State | Output                                                      |
| :-----------: | :---: | :--------: | ----------------------------------------------------------- |
|    locked     | coin  |  unlocked  | unlocks the turnstile so that the customer can push through |
|    locked     | push  |   locked   | none                                                        |
|   unlocked    | coin  |  unlocked  | none                                                        |
|   unlocked    | push  |   locked   | when the customer has pushed through, locks the turnstile   |

---

### Summary

- the `state pattern` allows an object to have many different behaviours that are based on its internal state
- unlike a procedural state machine, the `state pattern` represents state as a full-blown class
- the context gets its behaviour by delegating to the current state object it is composed with
- by `encapsulating` each state into a class, we localise any changes that will need to be made
- the `state and strategy patterns` have the same class diagram, but they differ in intent
- `strategy pattern` typically configures context classes with a behaviour or algorithm
- `state pattern` allows a context to change its behaviour as the state of the context changes
- `state transitions` can be controlled by the state classes or by the context classes
- using the `state pattern` will typically result in a greater number of classes in your design
- state classes may be shared among context instances
