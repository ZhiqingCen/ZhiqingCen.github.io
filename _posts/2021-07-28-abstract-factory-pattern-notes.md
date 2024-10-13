---
title: Abstract Factory Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_define an interface or abstract class for creating families of related (or dependent) objects but without specifying their concrete sub-classes_**

lets a class returns a factory of classes, so, this is the reason that Abstract Factory Pattern is one level higher than the Factory Pattern

- Benefits
  - Abstract Factory Pattern **isolates the client code** from concrete(implementation) classes
  - It **eases** the **exchanging of object families**
  - It **promotes consistency** among objects
- Usage
  - When the system needs to be **independent** of how its object are **created, composed, and represented**
  - When the `family` of related objects has to be used together, then this **constraint needs to be enforced**
  - When you want to provide a `library` of objects that does **not show implementations** and **only reveals interfaces**
  - When the `system` needs to be **configured with one of a multiple family of objects**

## Implementation

1. Create an interface

   ```java
   public interface Shape {
       void draw();
   }
   ```

2. Create `RoundedRectangle`, `RoundedSquare`, `Rectangle` classes implementing `Shape` interface

   ```java
   public class RoundedRectangle implements Shape {
       @Override
       public void draw() {
           System.out.println("Inside RoundedRectangle::draw() method.");
       }
   }
   ```

   ```java
   public class RoundedSquare implements Shape {
       @Override
       public void draw() {
           System.out.println("Inside RoundedSquare::draw() method.");
       }
   }
   ```

   ```java
   public class Rectangle implements Shape {
       @Override
       public void draw() {
           System.out.println("Inside Rectangle::draw() method.");
       }
   }
   ```

3. Create `AbstractFactory` Abstract class to get factories for Normal and Rounded Shape Objects

   ```java
   public abstract class AbstractFactory {
       abstract Shape getShape(String shapeType) ;
   }
   ```

4. Create `ShapeFactory`, `RoundedShapeFactory` Factory classes extending AbstractFactory to generate object of concrete class based on given information

   ```java
   public class ShapeFactory extends AbstractFactory {
       @Override
       public Shape getShape(String shapeType){
           if (shapeType.equalsIgnoreCase("RECTANGLE")) {
               return new Rectangle();
           } else if (shapeType.equalsIgnoreCase("SQUARE")) {
               return new Square();
           }
           return null;
       }
   }
   ```

   ```java
   public class RoundedShapeFactory extends AbstractFactory {
       @Override
       public Shape getShape(String shapeType){
           if(shapeType.equalsIgnoreCase("RECTANGLE")){
               return new RoundedRectangle();
           }else if(shapeType.equalsIgnoreCase("SQUARE")){
               return new RoundedSquare();
           }
           return null;
       }
   }
   ```

5. Create `FactoryProducer` Factory generator/producer class to get factories by passing an information such as Shape

   ```java
   public class FactoryProducer {
       public static AbstractFactory getFactory(boolean rounded){
           if(rounded){
               return new RoundedShapeFactory();
           }else{
               return new ShapeFactory();
           }
       }
   }
   ```

6. Create demo `AbstractFactoryPatternDemo` class

   ```java
   public class AbstractFactoryPatternDemo {
       public static void main(String[] args) {
           AbstractFactory shapeFactory = FactoryProducer.getFactory(false);
           Shape shape1 = shapeFactory.getShape("RECTANGLE");
           shape1.draw();
           Shape shape2 = shapeFactory.getShape("SQUARE");
           shape2.draw();

           AbstractFactory shapeFactory1 = FactoryProducer.getFactory(true);
           Shape shape3 = shapeFactory1.getShape("RECTANGLE");
           shape3.draw();
           Shape shape4 = shapeFactory1.getShape("SQUARE");
           shape4.draw();
       }
   }
   ```

## Notes

- Intent
  - Abstract Factory is a creational design pattern that lets you produce **families of related objects** without specifying their concrete classes
- Problem
  - Imagine that you’re creating a furniture shop simulator. Your code consists of classes that represent:
    - A family of related products, say: `Chair + Sofa + CoffeeTable`
    - Several variants of this family
    - For example, products `Chair + Sofa + CoffeeTable` are available in these **variants**

---

### Structure

1. **Abstract Products** declare interfaces for a set of distinct but related products which make up a product family
2. **Concrete Products** are various implementations of abstract products, grouped by variants. Each abstract product (chair/sofa) must be implemented in all given variants (Victorian/Modern)
3. The **Abstract Factory** interface declares a set of methods for creating each of the abstract products
4. **Concrete Factories** implement creation methods of the abstract factory. Each concrete factory corresponds to a specific variant of products and creates only those product variants.
5. The **Client** can work with any concrete factory/product variant, as long as it communicates with their objects via abstract interfaces
