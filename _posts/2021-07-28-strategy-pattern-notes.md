---
title: Strategy Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_defines a family of functionality, encapsulate each one, and make them interchangeable_**

- Benefits
  - It provides a substitute to `subclassing`
  - It defines each behaviour within its own class, **eliminating the need for conditional statements**
  - It makes it easier to extend and incorporate new behaviour **without changing the application**
- Usage
  - When the multiple classes **differ only in their behaviours**
    - e.g. Servlet API
  - It is used when you need **different variations** of an algorithm

## Implementation

1. Create a `Strategy` interface

   ```java
   public interface Strategy {
       public float calculation(float a, float b);
   }
   ```

2. Create a `Addition`, `Subtraction`, `Multiplication` classes that will implement `Strategy` interface

   ```java
   public class Addition implements Strategy {
       @Override
       public float calculation(float a, float b) {
           return a + b;
       }
   }
   ```

   ```java
   public class Subtraction  implements Strategy {
       @Override
       public float calculation(float a, float b) {
           return a - b;
       }
   }
   ```

   ```java
   public class Multiplication implements Strategy {
       @Override
       public float calculation(float a, float b) {
           return a * b;
       }
   }
   ```

3. Create a `Context` class that will ask from `Strategy` interface to execute the type of strategy

   ```java
   public class Context {
       private Strategy strategy;

       public Context(Strategy strategy) {
           this.strategy = strategy;
       }

       public float executeStrategy(float num1, float num2) {
           return strategy.calculation(num1, num2);
       }
   }
   ```

4. Create a `StartegyPatternDemo` class

   ```java
   import java.io.BufferedReader;
   import java.io.IOException;
   import java.io.InputStreamReader;

   public class StrategyPatternDemo {

       public static void main(String[] args) throws NumberFormatException, IOException {

           BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
           System.out.print("Enter the first value: ");
           float value1=Float.parseFloat(br.readLine());
           System.out.print("Enter the second value: ");
           float value2=Float.parseFloat(br.readLine());
           Context context = new Context(new Addition());
           System.out.println("Addition = " + context.executeStrategy(value1, value2));

           context = new Context(new Subtraction());
           System.out.println("Subtraction = " + context.executeStrategy(value1, value2));

           context = new Context(new Multiplication());
           System.out.println("Multiplication = " + context.executeStrategy(value1, value2));
       }

   }
   ```

## Notes

- enables "change behaviour" at run-time
- Motivation
  - Need a way to adapt the behaviour of an algorithm at runtime
- Intent
  - Define a family of algorithms, encapsulate each one, and make them interchangeable
  - Strategy pattern is a behavioural design pattern that lets the algorithm vary independently from the context class using it
- Applicability
  - Many related classes differ in their behaviour
  - A context class can benefit from different variants of an algorithm
  - A class defines many behaviours, and these appears as multiple conditional statements (e.g., if or switch). Instead, move each conditional branch into their own concrete strategy class
- Benefits
  - Uses `composition` over `inheritance` which allows better **_decoupling_** between the behaviour and context class that uses the behaviour
- Drawbacks
  - Increases the number of objects
  - Client must be aware of different strategies
