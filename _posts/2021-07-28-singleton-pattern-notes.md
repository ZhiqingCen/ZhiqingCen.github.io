---
title: Singleton Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**define a class that has only one instance and provides a global point of access to it**

- two forms
  - **Early Instantiation**: creation of instance at load time
  - **Lazy Instantiation**: creation of instance when required
- Benefits
  - **Saves memory** because `object` is **not created at each request**, only single instance is **reused** again and again
- Usage
  - mostly used in **multi-threaded** and **database** applications
  - It is used in logging, caching, thread pools, configuration settings etc

## Implementation

1. Create `SingleObject` Singleton Class with **private constructor**

   ```java
   public class SingleObject {
       //create an object of SingleObject
       private static SingleObject instance = new SingleObject();

       //make the constructor private so that this class cannot be instantiated
       private SingleObject(){}

       //Get the only object available
       public static SingleObject getInstance() { return instance; }

       public void showMessage() {
           System.out.println("Hello World!");
       }
   }
   ```

2. Create demo class

   ```java
   public class SingletonPatternDemo {
       public static void main(String[] args) {
           //illegal construct
           //Compile Time Error: The constructor SingleObject() is not visible
           // SingleObject object = new SingleObject();

           //Get the only object available
           SingleObject object = SingleObject.getInstance();
           object.showMessage();
       }
   }
   ```

## Notes

- intent
  - Singleton is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance
- Problem: A client wants to,
  - ensure that a class has just a **single instance**
  - provide a **global access point** to that **instance**

---

### Structure

- Add a `private static field` to the class for storing the singleton instance
- Declare a `public static creation method` for getting the singleton instance
  - Implement **“lazy initialization”** inside the `static method`
    - It should create a **new object** on its first call and put it into the static field
    - The method should always return that instance on all **subsequent calls**
- Make the `constructor` of the class `private`
  - The **static method** of the class will still be able to call the constructor, but not the other objects
- In a `client`, call singleton’s **static creation method** to **access the object**
