---
title: Observer Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_just define a one-to-many dependency so that when one object changes state, all its dependents are notified and updated automatically_**

- Benefits
  - It describes the **coupling** between the objects and the `observer`
  - It provides the support for **broadcast-type communication**
- Usage
  - When the change of a `state` in one object must be **reflected** in another object **without** keeping the objects **tight coupled**
  - When the framework we writes and needs to be **enhanced in future** with new `observers` with **minimal changes**

## Implementation - define observer & observable

1. Create `Subject` (Observable) class

   ```java
   import java.util.ArrayList;
   import java.util.List;

   public class Subject {
       private List<Observer> observers = new ArrayList<Observer>();
       private int state;

       public int getState() {
           return state;
       }

       public void setState(int state) {
           this.state = state;
           notifyAllObservers();
       }

       public void attach(Observer observer){
           observers.add(observer);
       }

       public void notifyAllObservers(){
           for (Observer observer : observers) {
               observer.update();
           }
       }
   }
   ```

2. Create `Observer` class

   ```java
   public abstract class Observer {
       protected Subject subject;
       public abstract void update();
   }
   ```

3. Create concrete classes, `BinaryObserver`, `OctalObserver`, `HexaObserver` that extends `Observer` class

   ```java
   public class BinaryObserver extends Observer{
       public BinaryObserver(Subject subject){
           this.subject = subject;
           this.subject.attach(this);
       }

       @Override
       public void update() {
           System.out.println( "Binary String: " + Integer.toBinaryString( subject.getState() ) );
       }
   }
   ```

   ```java
   public class OctalObserver extends Observer{
       public OctalObserver(Subject subject){
           this.subject = subject;
           this.subject.attach(this);
       }

       @Override
       public void update() {
           System.out.println( "Octal String: " + Integer.toOctalString( subject.getState() ) );
       }
   }
   ```

   ```java
   public class HexaObserver extends Observer{
       public HexaObserver(Subject subject){
           this.subject = subject;
           this.subject.attach(this);
       }

       @Override
       public void update() {
           System.out.println( "Hex String: " + Integer.toHexString( subject.getState() ).toUpperCase() );
       }
   }
   ```

4. Create `ObserverPatternDemo` class

   ```java
   public class ObserverPatternDemo {
       public static void main(String[] args) {
           Subject subject = new Subject();

           new HexaObserver(subject);
           new OctalObserver(subject);
           new BinaryObserver(subject);

           System.out.println("First state change: 15");
           subject.setState(15);
           System.out.println("Second state change: 10");
           subject.setState(10);
       }
   }
   ```

## Implementation - use libraries

1. Create a `ResponseHandler1`, `ResponseHandler2` classes the will implement the `java.util.Observer` interface

   ```java
   import java.util.Observable;
   import java.util.Observer;

   public class ResponseHandler1 implements Observer {
       private String resp;
       public void update(Observable obj, Object arg) {
           if (arg instanceof String) {
               resp = (String) arg;
               System.out.println("\nReceived Response: " + resp);
           }
       }
   }
   ```

   ```java
   import java.util.Observable;
   import java.util.Observer;

   public class ResponseHandler2 implements Observer {
       private String resp;
       public void update(Observable obj, Object arg) {
           if (arg instanceof String) {
               resp = (String) arg;
               System.out.println("\nReceived Response: " + resp);
           }
       }
   }
   ```

2. Create an `EventSource` class that will extend the `java.util.Observable` class

   ```java
   import java.io.BufferedReader;
   import java.io.IOException;
   import java.io.InputStreamReader;
   import java.util.Observable;

   public class EventSource extends Observable implements Runnable {
       @Override
       public void run() {
           try {
               final InputStreamReader isr = new InputStreamReader(System.in);
               final BufferedReader br = new BufferedReader(isr);
               while (true) {
                   String response = br.readLine();
                   setChanged();
                   notifyObservers(response);
               }
           }
           catch (IOException e) {
               e.printStackTrace();
           }
       }
   }
   ```

## Notes

- is used to implement distributed `event handling` systems, in "event driven" programming
- In the observer pattern
  - an `object`, called the `subject` / observable / publisher , maintains a list of its dependents, called `observers` / subscribers
  - notifies the `observers` automatically of any **state changes** in the `subject`, usually by calling one of their methods
- The Observer Pattern defines a **_one-to-many_** dependency between objects so that when one object (subject) changes state, all of its dependents (observers) are notified and updated automatically.
- The aim should be to,
  - define a one-to-many dependency between objects **without** making the objects **tightly coupled**
  - **automatically** notify/update an **open-ended** number of `observers` (dependent objects) when the `subject` changes state
  - be able to **dynamically** add and remove `observers`
- possible solution
  - Define Subject and Observer interfaces, such that when a subject changes state, all registered observers are notified and updated automatically.
- The responsibility of
  - a `subject` is to maintain a list of observers and to notify them of state changes by calling their `update()` operation.
  - `observers` is to register (and unregister) themselves on a subject (to get notified of state changes) and to update their state when they are notified.
- This makes subject and observers **loosely coupled**
- Observers can be **added** and **removed** independently at **run-time**
- This notification-registration interaction is also known as **publish-subscribe**

---

limitations

- `Observable` is a class, not an interface
- `Observable` protects crucial methods, the `setChanged()` method is protected
- we can’t call `setChanged()` unless we subclass `Observable`! `Inheritance` is must, bad design
- we can’t add on the `Observable` behaviour to an existing class that already extends another `superclass`
- there isn’t an `Observable` interface, for a proper custom implementation

---

passing data

- The `Subject` needs to pass (change) data while notifying a change to an `Observer`
- Push data
  - Subject passes the changed data to its observers
    - eg. `update(data1,data2,...)`
  - All observers must implement the above update method.
- Pull data
  - Subject passes reference to itself to its observers, and the observers need to get (pull) the required data from the subject
    - eg. `update(this)`
  - Subject needs to provide the required access methods for its observers.
    - eg. `public double getTemperature();`

---

Advantages:

- Avoids **tight coupling** between `Subject` and its `Observers`
- This allows the `Subject` and its `Observers` to be at different levels of abstractions in a system
- **Loosely coupled** objects are easier to maintain and reuse
- Allows **dynamic** registration and deregistration

Be careful:

- A change in the `subject` may result in a chain of updates to its `observers` and in turn their dependent objects – resulting in a **complex update behaviour**
- Need to properly manage such dependencies

---

### Summary

- the observer pattern defines a **one-to-many relationship** between Objects
- `subjects`, or as we also known them, `observables`, update `observers` using a common interface
- `observers` are **loosely coupled** in that the `observable` knows nothing about them, other than that they implement the `observer` interface
- you can push or `pull` data from the `observable` when using the pattern (pull is considered more "correct")
- don't depend on a specific order of notification for your `observers`
- java has several implementations of the observer pattern, including the general purpose `java.util.Observable`
- watch out for issues with the `java.util.Observable` implementation
- don't be afraid to create your own `observable` implementation if needed
- swing makes heavy use of the observer pattern, as do many GUI frameworks
- you'll also find the pattern in many other places, including JavaBeans and RMI
