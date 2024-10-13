---
title: Template Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_just define the skeleton of a function in an operation, deferring some steps to its subclasses_**

- Benefits
  - It is very common technique for **reusing the code**, this is only the main benefit of it
- Usage
  - It is used when the common behaviour among sub-classes should be moved to a single common class by **avoiding the duplication**

## Implementation

1. Create `Game`(Template) abstract class with a template method being `final`

   ```java
   public abstract class Game {
       abstract void initialize();
       abstract void startPlay();
       abstract void endPlay();

       //template method
       public final void play(){
           initialize();
           startPlay();
           endPlay();
       }
   }
   ```

2. Create `Cricket`, `Football` classes extending `Game` class

   ```java
   public class Cricket extends Game {
       @Override
       void initialize() {
           System.out.println("Cricket Game Initialized! Start playing.");
       }

       @Override
       void startPlay() {
           System.out.println("Cricket Game Started. Enjoy the game!");
       }

       @Override
       void endPlay() {
           System.out.println("Cricket Game Finished!");
       }
   }
   ```

   ```java
   public class Football extends Game {
       @Override
       void initialize() {
           System.out.println("Football Game Initialized! Start playing.");
       }

       @Override
       void startPlay() {
           System.out.println("Football Game Started. Enjoy the game!");
       }

       @Override
       void endPlay() {
           System.out.println("Football Game Finished!");
       }
   }
   ```

3. Use the Game's template method play() to demonstrate a defined way of playing game
   ```java
   public class TemplatePatternDemo {
       public static void main(String[] args) {
           Game game = new Cricket();
           game.play();
           System.out.println();
           game = new Football();
           game.play();
       }
   }
   ```

## Notes

- "Define the skeleton of an algorithm in an operation, deferring some steps to subclasses.Template Method lets subclasses redefine certain steps of an algorithm
  without changing the algorithm's structure." [GoF]
- A template Method defines the **skeleton**(structure) of a **behaviour**(by implementing the invariant parts)
- A template Method calls **primitive operations**, that could be implemented by sub classes OR has default implementations in an abstract super class
- `Subclasses` can **redefine only certain parts of a behaviour** without changing the other parts or the structure of the behaviour
- `Subclasses` **do not control the behaviour** of a `parent` class, a `parent` class calls the operations of a `subclass` and not the other way around
- Inversion of control:
  - when using a **library**(reusable classes), we call the code we want to reuse
  - when using a **framework**(like Template Pattern), we write `subclasses` and `implement` the variant code the framework calls
- Template pattern implement the **common(invariant) parts of a behaviour** once "and leave it up to subclasses to implement the behaviour that can vary"[GoF, p326]
- Invariant behaviour is in one class (localised)

---

### Structure

- "To reuse an abstract class effectively, subclass writers must understand which operations are designed for overriding." [GoF, p328]
- `Primitive operations`: operations that have **default implementations** or **must be** implemented by `sub-classes`
- `Final operations`: concrete operations that **cannot be overridden** by `sub-classes`
- `Hook operations`: concrete operations that **do nothing by default** and can be redefined by `subclasses` if necessary
  - This gives `subclasses` the ability to **“hook into” the algorithm** at various points, if they wish; a `subclass` is also **free to ignore the hook**

---

### Template Vs Strategy Patterns

- **Template** Method works at the class level, so it’s **static**
- **Strategy** works on the object level, letting you **switch behaviours at runtime**
- **Template** Method is based on `inheritance`: it lets you alter parts of an **algorithm** by extending those parts in `subclasses`
- **Strategy** is based on `composition`: you can alter parts of the **object’s behaviour** by supplying it with different strategies that correspond to that **behaviour at runtime**
