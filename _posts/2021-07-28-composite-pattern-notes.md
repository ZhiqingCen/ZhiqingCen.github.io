---
title: Composite Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_allow clients to operate in generic manner on objects that may or may not represent a hierarchy of objects_**

- Benefits
  - It defines **class hierarchies** that contain **primitive** and **complex** objects
  - It makes easier to you to **add new kinds of components**
  - It provides **flexibility of structure** with manageable class or interface
- Usage
  - When you want to represent a full or partial **hierarchy** of objects
  - When the **responsibilities** are needed to be added **dynamically** to the individual objects without affecting other objects, where the responsibility of object may vary from time to time
- elements
  - `Component`
    - Declares interface for objects in composition
    - Implements default behaviour for the interface common to all classes as appropriate
    - Declares an interface for accessing and managing its child components
  - `Leaf`
    - Represents leaf objects in composition. A leaf has no children
    - Defines behaviour for primitive objects in the composition
  - `Composite`
    - Defines behaviour for components having children
    - Stores child component
    - Implements child related operations in the component interface
  - `Client`
    - Manipulates objects in the composition through the component interface

## Implementation - Uniformity

1. Create `Component` interface

   ```java
   public interface Component {
       public String nameString();
       public double calculateCost();

       public boolean add(Component child);
       public boolean remove(Component child);
   }
   ```

2. Create `Leaf` class that will implement `Component` interface

   ```java
   public class Leaf implements Component {
       private String name;
       private double cost;

       public Leaf(String name, double cost) {
           super();
           this.name = name;
           this.cost = cost;
       }

       @Override
       public String nameString() {
           return this.name;
       }

       @Override
       public double calculateCost() {
           return this.cost;
       }

       @Override
       public boolean add(Component child) {
           return false;
       }

       @Override
       public boolean remove(Component child) {
           return false;
       }
   }
   ```

3. Create `Composite` class that will implement `Component` interface

   ```java
   import java.util.ArrayList;

   public class Composite implements Component {
       private String name;
       private double cost;
       ArrayList<Component>  children = new ArrayList<Component>();

       public Composite(String name, double cost) {
           super();
           this.name = name;
           this.cost = cost;
       }

       @Override
       public double calculateCost() {
           double answer = this.getCost();
           for(Component c : children) {
               answer += c.calculateCost();
           }
           return answer;
       }

       @Override
       public String nameString() {
           String answer = "[" + this.getName()  + " ";
           for(Component c : children) {
               answer = answer + " " + c.nameString();
           }
           answer = answer + "]";
           return answer;
       }

       @Override
       public boolean add(Component child) {
           children.add(child);
           return true;
       }

       @Override
       public boolean remove(Component child) {
           children.remove(child);
           return true;
       }
   }
   ```

4. Create demo `Uniformity` class

   ```java
   public class Uniformity {
       public static void main(String[] args) {
           Component mainboard = new Composite("Mainboard", 100);
           Component processor = new Leaf("Processor", 450);
           Component memory    = new Leaf("Memory", 80);
           mainboard.add(processor);
           mainboard.add(memory);

           Component chasis = new Composite("Chasis", 75);
           chasis.add(mainboard);

           Component disk = new Leaf("Disk", 50);
           chasis.add(disk);

           System.out.println("[0] "+ processor.nameString());
           System.out.println("[0] "+ processor.calculateCost());

           System.out.println("[1] "+ mainboard.nameString());
           System.out.println("[1] "+ mainboard.calculateCost());

           System.out.println("[2] "+ chasis.nameString());
           System.out.println("[2] "+ chasis.calculateCost());
       }
   }
   ```

## Implementation - Type Safety

1. Create `Employee` interface

   ```java
   public interface Employee {
       public void showEmployeeDetails();
   }
   ```

2. Create `Developer`, `Manager` (`Leaf`) class that will implement `Employee` interface

   ```java
   public class Developer implements Employee {
       private String name;
       private long empId;
       private String position;

       public Developer(long empId, String name, String position) {
           this.empId = empId;
           this.name = name;
           this.position = position;
       }

       @Override
       public void showEmployeeDetails() {
           System.out.println(empId + " " + name);
       }
   }
   ```

   ```java
   public class Manager implements Employee {
       private String name;
       private long empId;
       private String position;

       public Manager(long empId, String name, String position) {
           this.empId = empId;
           this.name = name;
           this.position = position;
       }

       @Override
       public void showEmployeeDetails() {
           System.out.println(empId + " " + name);
       }
   }
   ```

3. Create `CompanyDirectory` (`Composite`) class that will implement `Employee` interface

   ```java
   import java.util.ArrayList;
   import java.util.List;

   public class CompanyDirectory implements Employee {
       private List<Employee> employeeList = new ArrayList<Employee>();

       @Override
       public void showEmployeeDetails() {
           for (Employee emp : employeeList) {
               emp.showEmployeeDetails();
           }
       }

       public void addEmployee(Employee emp) {
           employeeList.add(emp);
       }

       public void removeEmployee(Employee emp) {
           employeeList.remove(emp);
       }
   }
   ```

4. Create demo `Company` class

   ```java
   public class Company {
       public static void main (String[] args) {
           Developer dev1 = new Developer(100, "Lokesh Sharma", "Pro Developer");
           Developer dev2 = new Developer(101, "Vinay Sharma", "Developer");
           CompanyDirectory engDirectory = new CompanyDirectory();
           engDirectory.addEmployee(dev1);
           engDirectory.addEmployee(dev2);

           Manager man1 = new Manager(200, "Kushagra Garg", "SEO Manager");
           Manager man2 = new Manager(201, "Vikram Sharma", "Kushagra's Manager");

           CompanyDirectory accDirectory = new CompanyDirectory();
           accDirectory.addEmployee(man1);
           accDirectory.addEmployee(man2);

           CompanyDirectory directory = new CompanyDirectory();
           directory.addEmployee(engDirectory);
           directory.addEmployee(accDirectory);
           directory.showEmployeeDetails();
       }
   }
   ```

## Notes

- `Tree` structures are normally used to represent part-whole hierarchies
  - A **multiway** `tree` structure stores a collection of say Components at each node (children below), to store `Leaf` objects and `Composite` (subtree) objects
- A `Leaf` object performs operations directly on the object
- A `Composite` object performs operations on its children, and if required, collects return values and derives the required answers

---

### 2 approach

- `Uniformity`: include all child-related operations in the `Component` interface
  - include all child-related operations in the `Component` interface, this means the `Leaf` class needs to implement these methods with “do nothing” or “throw exception”
  - a client can treat both `Leaf` and `Composite` objects **uniformly**
  - we **loose type safety** because Leaf and Composite types are not cleanly separated
  - useful for **dynamic structures** where children types change dynamically(from `Leaf` to `Composite` and vice versa), and a client needs to perform child-related operations regularly
- `Type Safety`: only define child-related operations in the `Composite` class
  - only define child-related operations in the `Composite` class
  - the type system **enforces type constraints**, so a client cannot perform child-related operations on a `Leaf` object
  - a client needs to treat `Leaf` and `Composite` objects **differently**
  - useful for **static structures** where a client doesn’t need to perform child-related operations on “unknown” objects of type `Component`

---

### Summary

- The Composite Pattern provides a structure to hold both **individual objects** and **composites**
- The Composite Pattern allows clients to treat composites and individual objects **uniformly**
- A `Component` is any object in a Composite structure
  - `Components` may be other `composites` or `leaf` nodes
- There are many design tradeoffs in implementing Composite, you need to balance `transparency`/`uniformity` and `type safety` with your needs
