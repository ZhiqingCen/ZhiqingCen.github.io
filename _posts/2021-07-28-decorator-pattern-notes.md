---
title: Decorator Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_attach a flexible additional responsibilities to an object dynamically_**

The Decorator Pattern uses `composition` **instead** of `inheritance` to extend the functionality of an object **at runtime**

- Benefits
  - It provides **greater flexibility** than **static** `inheritance`
  - It **enhances the extensibility** of the `object`, because changes are made by coding new classes
  - It simplifies the coding by allowing you to **develop a series of functionality** from **targeted classes** instead of coding all of the behaviour into the object
- Usage
  - When you want to **transparently** and **dynamically** add **responsibilities** to `objects` without affecting other `objects`
  - When you want to **add responsibilities** to an `object` that you may want to change in **future**
  - **Extending functionality** by `sub-classing` is no longer practical

## Implementation

1. Create `Shape` interface

   ```java
   public interface Shape {
       void draw();
   }
   ```

2. Create `Rectangle`, `Circle` class that will implement `Shape` interface

   ```java
   public class Rectangle implements Shape {
       @Override
       public void draw() {
           System.out.println("Shape: Rectangle");
       }
   }
   ```

   ```java
   public class Circle implements Shape {
       @Override
       public void draw() {
           System.out.println("Shape: Circle");
       }
   }
   ```

3. Create `ShapeDecorator` abstract class that will implement `Shape` interface

   ```java
   public abstract class ShapeDecorator implements Shape {
       protected Shape decoratedShape;

       public ShapeDecorator(Shape decoratedShape) {
           this.decoratedShape = decoratedShape;
       }

       public void draw() {
           decoratedShape.draw();
       }
   }
   ```

4. Create `RedShapeDecorator` class that extends `ShapeDecorator` class

   ```java
   public class RedShapeDecorator extends ShapeDecorator {
       public RedShapeDecorator(Shape decoratedShape) {
           super(decoratedShape);
       }

       @Override
       public void draw() {
           decoratedShape.draw();
           setRedBorder(decoratedShape);
       }

       private void setRedBorder(Shape decoratedShape) {
           System.out.println("Border Color: Red");
       }
   }
   ```

5. Create demo `RedShapeDecorator` class

   ```java
   public class DecoratorPatternDemo {
       public static void main(String[] args) {
           Shape circle = new Circle();
           Shape redCircle = new RedShapeDecorator(new Circle());
           Shape redRectangle = new RedShapeDecorator(new Rectangle());

           System.out.println("Circle with normal border");
           circle.draw();

           System.out.println("\nCircle of red border");
           redCircle.draw();

           System.out.println("\nRectangle of red border");
           redRectangle.draw();
       }
   }
   ```

## Notes

- "**Attach** additional **responsibilities** to an object **dynamically**. Decorators provide a flexible alternative to sub-classing for extending functionality." [GoF]
- **Decorator design patterns** allow us to selectively add **functionality to an object** (not the class) at runtime, based on the requirements
- Original class is **not** changed (Open-Closed Principle)
- `Inheritance` extends behaviours at **compile time**, additional functionality is bound to all the instances of that class for their life time
- The `decorator` design pattern prefers a **composition** over an inheritance, its a **structural pattern**, which provides a **wrapper** to the existing class
- Objects can be `decorated` **multiple times**, in different order, due to the **recursion** involved with this design pattern
- Do not need to implement all possible functionality in a single (complex) class
- Given that the `decorator` has the same `super-type` as the object it `decorates`, we can pass around a **decorated object** in place of the original (wrapped) object
- The `decorator` adds its own behaviour either **before** and/or **after** delegating to the object it `decorates` to do the rest of the job
