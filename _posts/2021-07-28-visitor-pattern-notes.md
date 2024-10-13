---
title: Visitor Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_adds new operations/behaviours to the existing objects, without modifying them_**

- Benefits
  - element object has to accept the visitor object so that visitor object handles the operation on the element object
  - way of **separating an algorithm** from an object structure on which it operates
  - able to **add new operations** to existing object structures **without modifying the structures**
- Usage
  - used when we have to perform an operation on a **group of similar kind of Objects**
  - can move the **operational logic** from the objects to another class

## Implementation

1. Create `ComputerPart` interface

   ```java
   public interface ComputerPart {
       public void accept(ComputerPartVisitor computerPartVisitor);
   }
   ```

2. Create `Keyboard`, `Monitor`, `Mouse`, `Computer` concrete class implementing `ComputerPart` interface

   ```java
   public class Keyboard implements ComputerPart {
       @Override
       public void accept(ComputerPartVisitor computerPartVisitor) {
           computerPartVisitor.visit(this);
       }
   }
   ```

   ```java
   public class Monitor implements ComputerPart {
       @Override
       public void accept(ComputerPartVisitor computerPartVisitor) {
           computerPartVisitor.visit(this);
       }
   }
   ```

   ```java
   public class Mouse implements ComputerPart {
       @Override
       public void accept(ComputerPartVisitor computerPartVisitor) {
           computerPartVisitor.visit(this);
       }
   }
   ```

   ```java
   public class Computer implements ComputerPart {
       ComputerPart[] parts;

       public Computer() {
           parts = new ComputerPart[] {new Mouse(), new Keyboard(), new Monitor()};
       }

       @Override
       public void accept(ComputerPartVisitor computerPartVisitor) {
           for (int i = 0; i < parts.length; i++) {
               parts[i].accept(computerPartVisitor);
           }
           computerPartVisitor.visit(this);
       }
   }
   ```

3. Create `ComputerPartVisitor` (visitor) interface

   ```java
   public interface ComputerPartVisitor {
       public void visit(Computer computer);
       public void visit(Mouse mouse);
       public void visit(Keyboard keyboard);
       public void visit(Monitor monitor);
   }
   ```

4. Create `ComputerPartDisplayVisitor` concrete (visitor) class implementing `ComputerPartVisitor` class

   ```java
   public class ComputerPartDisplayVisitor implements ComputerPartVisitor {
       @Override
       public void visit(Computer computer) {
           System.out.println("Displaying Computer.");
       }

       @Override
       public void visit(Mouse mouse) {
           System.out.println("Displaying Mouse.");
       }

       @Override
       public void visit(Keyboard keyboard) {
           System.out.println("Displaying Keyboard.");
       }

       @Override
       public void visit(Monitor monitor) {
           System.out.println("Displaying Monitor.");
       }
   }
   ```

5. Create demo class
   ```java
   public class VisitorPatternDemo {
       public static void main(String[] args) {
           ComputerPart computer = new Computer();
           computer.accept(new ComputerPartDisplayVisitor());
       }
   }
   ```

## Notes

- It is one way to follow the `open/closed principle`
- A `visitor class` is created that implements all of the **appropriate specialisations** of the **virtual operation/method**
- The `visitor` takes the **instance reference as input**, and **implements the goal** (additional behaviour)
- Visitor pattern can be added to **public APIs**, allowing its clients to perform operations on a class **without having to modify the source**

---

- Applicability: Moving operations into visitor classes is beneficial when
  - **many unrelated operations** on an object structure are required
  - the classes that make up the object structure are **known** and **not expected to change**
  - **new operations** need to be **added frequently**
  - an **algorithm** involves several classes of the object structure, but it is desired to manage it **in one single location**
  - an **algorithm** needs to work **across several independent class hierarchies**
- Limitation:
  - extensions to the class hierarchy more difficult, as a **new class** typically **require a new `visit` method** to be added to each visitor

---

### Structure

- The Visitor pattern suggests that you place the new behaviour into a **separate class** called `visitor`, instead of trying to integrate it into existing classes
- The original object that had to perform the behaviour is now passed to one of the **visitor’s methods** as an argument, providing the method access to all necessary data contained within the object (see the example for more clarification)
- The `visitor` class need to define **a set of methods**, one for each type. For example, a city, a sightseeing place, an industry, etc
- The visitor pattern uses a technique called **“Double Dispatch”** to execute a suitable method on a given object(of different types)
  - An object **“accepts”** a `visitor` and tells it what visiting method should be executed
  - One additional method allows us to **add further behaviours without further altering the code**
