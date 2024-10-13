---
title: Builder Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_construct a complex object from simple objects using step-by-step approach_**

- Benefits
  - It provides **clear separation** between the **construction** and **representation** of an `object`
  - It provides **better control** over **construction process**
  - It supports to **change** the **internal representation** of `objects`
- Usage
  - It is mostly used when `object` **can't be created in single step **like in the de-serialisation of a complex object

## Implementation

1. Create `Packing` interface

   ```java
   public interface Packing {
       public String pack();
       public int price();
   }
   ```

2. Create `CD` abstract class implementing `Packing` interface

   ```java
   public abstract class CD implements Packing {
       public abstract String pack();
   }
   ```

3. Create `Company` abstract class extending `CD` abstract class

   ```java
   public abstract class Company extends CD {
       public abstract int price();
   }
   ```

4. Create `Sony`, `Samsung` class extending `Company` abstract class

   ```java
   public class Sony extends Company {
       @Override
       public int price() {
           return 20;
       }
       @Override
       public String pack() {
           return "Sony CD";
       }
   }
   ```

   ```java
   public class Samsung extends Company {
       @Override
       public int price() {
           return 15;
       }
       @Override
       public String pack() {
           return "Samsung CD";
       }
   }
   ```

5. Create `CDType` class

   ```java
   import java.util.ArrayList;
   import java.util.List;
   public class CDType {
       private List<Packing> items = new ArrayList<Packing>();

       public void addItem(Packing packs) {
           items.add(packs);
       }

       public void getCost() {
           for (Packing packs : items) {
               packs.price();
           }
       }

       public void showItems() {
           for (Packing packing : items) {
               System.out.print("CD name : "+packing.pack());
               System.out.println(", Price : "+packing.price());
           }
       }
   }
   ```

6. Create `CDBuilder`(Builder) class

   ```java
   public class CDBuilder {
       public CDType buildSonyCD(){
           CDType cds = new CDType();
           cds.addItem(new Sony());
           return cds;
       }

       public CDType buildSamsungCD(){
           CDType cds = new CDType();
           cds.addItem(new Samsung());
           return cds;
       }
   }
   ```

7. Create demo `BuilderDemo` class

   ```java
   public class BuilderDemo{
       public static void main(String args[]){
           CDBuilder cdBuilder = new CDBuilder();
           CDType cdType1 = cdBuilder.buildSonyCD();
           cdType1.showItems();

           CDType cdType2 = cdBuilder.buildSamsungCD();
           cdType2.showItems();
       }
   }
   ```

## Notes

- The Builder pattern suggests that you **extract** the **object construction code** out of its own class and **move it to** separate objects called `builders`
- Intent
  - Builder is a creational design pattern that lets you **construct complex objects step by step**, the pattern allows you to produce different types and representations of an object using the same construction code
- Problem
  - Imagine a **complex object** that requires laborious, **step-by-step initialization/construction** of many fields and nested objects
  - Such initialization/construction code is usually buried inside a monstrous `constructor` with **lots of parameters**
  - Or even worse: **scattered** all over the client code
- The Builder **doesn’t allow** other objects to **access** the product **while it’s being built**
- `Director`: The `director` class defines the **order of executing the building steps**, while the `builder` **provides the implementation** for those steps

---

### Structure

- The **Builder** interface declares product construction steps that are common to all types of builders
- **Concrete Builders** provide different implementations of the construction steps, may produce products that don’t follow the common interface
- **Products** are resulting objects, products constructed by different builders don’t have to belong to the same class hierarchy or interface
- The **Director** class defines the order in which to call construction steps, so you can create and reuse specific configurations of products
- The **Client** must associate one of the builder objects with the director

---

### Relations with Other Patterns

- Many designs start by using `Factory Method` (**less complicated** and **more customisable** via `subclasses`) and evolve toward `Abstract Factory`, or `Builder` (**more flexible**, but **more complicated**)
- `Builder` focuses on **constructing complex objects step by step**
  - lets you run some **additional construction steps** before fetching the product
- `Abstract Factory` specialises in **creating families of related objects**
  - returns the product immediately
