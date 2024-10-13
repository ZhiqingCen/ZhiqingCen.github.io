---
title: Factory Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_define an interface or abstract class for creating an object but let the subclasses decide which class to instantiate_**

`subclasses` are responsible to create the instance of the class

- Benefits
  - Factory Method Pattern allows the `sub-classes` to **choose the type** of objects to create
  - It promotes the **loose-coupling** by **eliminating the need** to **bind application-specific classes into the code**. That means the code interacts solely with the resultant `interface` or `abstract class`, so that it will work with any classes that implement that `interface` or that extends that `abstract class`
- Usage
  - When a class **doesn't know** what `sub-classes` will be **required to create**
  - When a class wants that its `sub-classes` **specify the objects to be created**
  - When the `parent` classes choose the creation of objects to its `sub-classes`

## Implementation

1. Create an interface

   ```java
   public interface Shape {
       void draw();
   }
   ```

2. Create `Rectangle`, `Square`, `Circle` classes implementing `Shape` interface

   ```java
   public class Rectangle implements Shape {
       @Override
       public void draw() {
           System.out.println("Inside Rectangle::draw() method.");
       }
   }
   ```

   ```java
   public class Square implements Shape {
       @Override
       public void draw() {
           System.out.println("Inside Square::draw() method.");
       }
   }
   ```

   ```java
   public class Circle implements Shape {
       @Override
       public void draw() {
           System.out.println("Inside Circle::draw() method.");
       }
   }
   ```

3. Create `ShapeFactory` Factory class to generate object of concrete class based on given information

   ```java
   public class ShapeFactory {
       //use getShape method to get object of type shape
       public Shape getShape(String shapeType) {
           if(shapeType == null){ return null; }
           if(shapeType.equalsIgnoreCase("CIRCLE")) {
               return new Circle();
           } else if(shapeType.equalsIgnoreCase("RECTANGLE")) {
               return new Rectangle();
           } else if(shapeType.equalsIgnoreCase("SQUARE")) {
               return new Square();
           }
           return null;
       }
   }
   ```

4. Create demo `FactoryPatternDemo` class

   ```java
   public class FactoryPatternDemo {
      public static void main(String[] args) {
         ShapeFactory shapeFactory = new ShapeFactory();

         Shape shape1 = shapeFactory.getShape("CIRCLE");
         shape1.draw();

         Shape shape2 = shapeFactory.getShape("RECTANGLE");
         shape2.draw();

         Shape shape3 = shapeFactory.getShape("SQUARE");
         shape3.draw();
      }
   }
   ```

## Notes

- Factory Method is a creational design pattern that uses factory methods to deal with the problem of creating objects **without** having to **specify the exact class** of the object that will be created
- Problem:
  - creating an object directly within the class that requires (uses) the object is **inflexible**
  - it **commits** the class to a particular object and
  - makes it **impossible to change** the instantiation independently from (without having to change) the class
- Possible Solution:
  - Define a **separate operation**(factory method) for creating an object
  - Create an object by calling a factory method
  - This enables writing of `subclasses` to change the way an object is created(to redefine which class to instantiate)

---

### Structure

1. The **Product** declares the interface, which is common to all objects that can be produced by the creator and its subclasses
2. **Concrete Products** are different implementations of the product interface
3. The **Creator** class declares the factory method that returns new product objects
4. **Concrete Creators** override the base factory method so it returns a different type of product
