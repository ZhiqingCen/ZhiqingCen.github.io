---
title: Java Notes
date: 2021-08-01 09:00:00 +1000
categories: [Programming, Java]
tags: [java, object-oriented programming, programming languages]
description: "Comprehensive notes on Java programming. Includes detailed explanations of core concepts like primitive and reference types, ArrayList, LinkedList, HashMap, and exception handling."
---

## keywords

**naming**

- `Class` -> PascalNamingConvention
    - first letter of every words upper case
- `Method` -> camelNamingConvention
    - camel case

```java
// System -> Class
// out -> field (belongs to class PrintStream)
// println -> method
System.out.println("Hello World!");
// shourtcut -> sout + Tab
```

`this`

- invoke current class constructor
- invoke current class method
- return the current class object
- pass an argument in the method call
- pass an argument in the constructor call

`static`

- a non-access modifier used for methods and attributes
- static attributes/methods can be access without creating an object of the class

## Data Types

### Compile & Execute

```console
// compile code
$ javac Filename.java

// run code
$ java PackageName.Filename
```

### Variables & Constant

`variables`

```java
// declare variables
int a = 0, z = 100;
long b = 250;
double c = 2.5;
char d = 'i';
String e = "Hello"
boolean f = true;

// update variables
a = 10;

// for large number, can use underscore
// 123456789 = 123_456_789, like 123,456,789 in real world
int number = 123_456_789;

// add suffix 'L'/'l' to represent as Long
long largeInteger = 3_123_456_789L;

// by default, java sees decimal numbers as double
// add suffix 'F'/'f' to represent as Float
float price = 10.99F;

// arithmetic expression
double result = (double)10 / (double)3;
int x = 1;
int y = x++;
x = 1;
int z = ++x;
System.out.println(x); // StadardOut: 2
System.out.println(y); // StadardOut: 1
System.out.println(z); // StadardOut: 2

```

`constants`

```java
// constant name in UpperCase, constant cannot be change
final float PI = 3.14F;
```

### Primitive & Reference Types

`Primitive Types` -> for storing simple values like numbers, strings and boolean

|  Type   | Bytes |                Range                 |
| :-----: | :---: | :----------------------------------: |
|  byte   |   1   |             [-128, 127]              |
|  short  |   2   |             [-32K, 32K]              |
|   int   |   4   |     [-2B, 2B],-2^31 to +(2^31)-1     |
|  long   |   8   | whole numbers without decimal points |
|  float  |   4   |     numbers with decimal points      |
| double  |   8   |     numbers with decimal points      |
|  char   |   2   |             A, B, C, ...             |
| boolean |   1   |             true / false             |


`Reference Types` -> for storing complex objects like data and email messages

```java
// Date in package java.util
import java.util.Date;

// declare variable
// new -> allocate memory
Date now = new Date();
// Date() by default returns current DateTime
now.getTime();
```

`Primitive Types` vs. `Reference Types`

```java
// Primitive
byte x = 1;
byte y = x;
x = 2;
System.out.println(y);
// StadardOut: 1

// Reference
// allocate some memory to store Point(x:1, y:1) object (eg. addr:100)
Point point1 = new Point(x:1, y:1); // allocate seperate part of memory for point1, store address of the point object (100)
Point point2 = point1; // allocate another memory for point2, store address of the point object (100)
point1.x = 2; // point1 & point2 reference the exact same point object
System.out.println(point2); // StadardOut: java.awt.Point[x=2,y=1]
```

### Casting

```java
// implicit casting: byte -> short -> int -> long -> float -> double
short x = 1;
int y = x + 2; // StadardOut: 3

// explicit casting
double x = 1.1;
int y = (int) x + 2; // StadardOut: 3

String x = "1";
Integer.parseInt(x); // convert string to integer
Short.parseShort(x); // concert string to short
Float.parseFloat(x); // concert string to float
Double.parseDouble(x); // concert string to double

int result = math.round(1.1F); // StadardOut: 1
int result = (int)math.ceil(1.1F); // StadardOut: 2
int result = (int)math.floor(1.1F); // StadardOut: 1
double result = math.random(); // generate random double between 0 and 1
int result = (int)math.round(math.random - 100);
int result = (int)(math.random - 100);
```

### Format Numbers

```java
// format as currency
NumberFormat currency = NumberFormat.getCurrencyInstance();
// return a string representation formatted as currency
String result = currency.format(1234567.891); // StadardOut: $1,234,567.89

// format as percentage
NumberFormat percent = NumberFormat.getPercentInstance();
// return a string representation formatted as percentage
String result = percent.format(0.1); // StadardOut: 10%

```

### String

`String` -> string in java are immutable

```java
// String -> in java.lang package
String message1 = new String("Hello World!");
String message2 = "Hello World";
String message3 = "Hello World" + "!";
String hi = "Hello \"Zhiqing\"";

// check & modify string
message3.startsWith("!"); // check if message3 startsWith "!", returns false
message3.endsWith("!"); // check if message3 endsWith "!", returns true
message3.length; // find length of string, returns 12
message3.indexOf("H"); // find index of char/string "H" in message3, returns 0, if not exist, returns -1
message3.replace("!", "~"); // replace all "!" in message3 with "~", does not modify original string, returns a new string
message3.trim(); // remove spaces from start and end of string
message3.toUpperCase(); // convert message3 to UpperCase
message3.toLowerCase(); // convert message3 to LowerCase

// string comparison, cannot use == or !=
String i = "hi";
// check if i is not "hi"
if (!i.equals("hi")) {
    System.out.println("i is not hi");
}
```

### Array

`array` -> fixed length

{% raw %}
```java
// boolean array by default initialise to false
// string array by default initialise to empty string
// declare an int array with 5 elements, initialise with zeros
int[] numbers = new int[5];
numbers[0] = 1;
numbers[1] = 2;
System.out.println(numbers); // StadardOut: address of array numbers

// string representation of array
System.out.println(Arrays.toString(numbers)); // StadardOut: [1, 2, 0, 0, 0]


int[] numberArray = {1, 2, 3, 4, 5};
numberArray.length; // length of array, returns 5
Arrays.sort(numberArray); // sort array

// 2D Arrays
int[][] numbers = new int[2][3];
numbers[0][0] = 1;
System.out.println(Arrays.deepToString(numbers)); // print multi-dimentional arrays, StadardOut: [[1, 0, 0], [0, 0, 0]]

int[][] numberArray = {{1, 2, 3}, {4, 5, 6}};
```
{% endraw %}

### ArrayList

`ArrayList` -> Dynamic sized arrays in Java that implement List interface

- has a regular array inside it, when an element is added, it is placed into the array
- if the array is not big enough, a new, larger array is created to replace the old one and the old one is removed

```java
// initialise ArrayList
import java.util.ArrayList;
List<Integer> a = new ArrayList<Integer>(); // List is interface
ArrayList<Integer> arrL = new ArrayList<Integer>(); // ArrayList is class
ArrayList<String> arrL2 = new ArrayList<String>();
ArrayList<Object> arrL3 = new ArrayList<>();

arrL2.add("apple"); // append
arrL2.add(1,  "watermelon"); // add to specific index
arrL2.set(0, "orange"); // update element by index
arrL2.get(0); // access element by index
arrL2.indexOf(); // get index of element
arrL2.remove(0); // remove element by index
arrL2.clear(); // remove all elements
arrL2.size(); // length of ArrayList
import java.util.Collections;
Collections.sort(arrL2); // sort ArrayList
```

### LinkedList

`LinkedList`

- the list has a link to the first container and each container has a link to the next container in the list
- to add an element to the list, the element is placed into a new container and that container is linked to one of the other containers in the list

```java
import java.util.LinkedList;
LinkedList<String> fruits = new LinkedList<String>();

fruits.add("apple"); // append
fruits.add(1, "apple"); // add to specific index
fruits.addFirst();
fruits.addLast();

fruits.set(1, "orange"); // update element by index

fruits.remove("apple"); // remove by elements
fruits.remove(0); // remove by index
fruits.removeFirst();
fruits.removeLast();

fruits.get(0); // access element by index
fruits.getFirst();
fruits.getLast();
```

### HashMap

`HashMap`

- not synchronised
    - not-thread safe and can’t be shared between many threads without proper synchronisation code
- allows one null key and multiple null values
- generally preferred over HashTable if thread synchronisation is not needed

`HashTable`

- synchronised
- doesn’t allow any null key or value

```java
import java.util.HashMap;
HashMap<String, String> capitalCities = new HashMap<String, String>();
HashMap<String, Integer> people = new HashMap<String, Integer>();

capitalCities.put("England", "London"); // add / update elements
capitalCities.get("England");
capitalCities.remove("England");
capitalCities.clear(); // remove all items
capitalCities.size();
capitalCities.keySet(); // get keys of HashMap
capitalCities.values(); // get values of HashMap
capitalCities.getKey();
capitalCities.getValue();
capitalCities.containsKey();
capitalCities.containsValue();

for (String i : capitalCities.keySet()) {
  System.out.println("key: " + i + " value: " + capitalCities.get(i));
}
```

### Read Input

```java
Scanner scanner = new Scanner(System.in);
System.out.print("Age: ");
byte age = scanner.nextByte(); // read in byte
System.out.println("You are" + age);
String name = scanner.next(); // read in string (stop at space)
String fullName = scanner.nextLine(); // read in string (stop at new line)
scanner.close(); // close scanner
```

## Conditional Statement

### If Statement

```java
// if statement
if (a == 0) {

} else if (a > 0) {

} else {

}

// ternary operation
int income = 120_000;
String className = income > 100_000 ? "First" : "Economy";
String className = "Economy";
if (income > 100_000) {
    className = "First"
}
```

### Switch Statement

```java
String role = "admin";

switch (role) {
    case "admin":
        System.out.println("You're an admin");
        break;

    case "moderator":
        System.out.println("You're a moderator");
        break;

    default:
        System.out.println("You're a guest");
}
```

### Loops

```java
// for loops
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// for each loop, can only loop forward
String[] fruits = {"apple", "banana", "cherry"};
for (String fruit : fruits) {
    System.out.println(fruit);
}

// while loop
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

// do while loop, execute do block before checking condition
int i = 0;
do {
    System.out.println(i);
} while (i < 5);
```

### Try-except Block

```java
// try-except block
try {

} catch (Exception e) {

}
```

## Methods

```java

```

## Enumeration

Advanced enum example

```java
enum CupSize {
    SMALL(25) {
        public String getDetail() {
            return "Use the clear glass cup";
        }
    },
    MEDIUM(40),
    LARGE(110);

    private int volume;

    private CupSize(int volume) {
        this.volume = volume;
    }

    public int getVolume() {
        return volume;
    }

    public String getDetail() {
        return "Use the blue cup";
    }
}

class Coffee {

    private CupSize cupSize;
    public Coffee() {}

    public Coffee (CupSize cupSize) {
        this.cupSize = cupSize;
    }

    public CupSize getCupSize () {
        return cupSize;
    }

    public void setCupSize (CupSize cupSize) {
        this.cupSize = cupSize;
    }

    @Override
    public String toString() {
        return "Coffee [cupSize: " + cupSize + ", volume: " + cupSize.getVolume() + " ]";
    }
}

public class MainForTestingEnum {
    public static void main(String[] args) {
        // using enum
        CupSize cup = CupSize.MEDIUM;
        System.out.println("Value of cup: " + cup + ", " + cup.getDetail()); // Value of cup: MEDIUM, Use the blue cup

        // using enum as a member variable inside coffee
        Coffee coffee1 = new Coffee(CupSize.SMALL);
        System.out.println(coffee1.toString() + ", " + coffee1.getCupSize().getDetail()); // Coffee [cupSize: SMALL, volume: 25 ], Use the clear glass cup
    }
}
```

## References

- course COMP2511 - Object-Oriented Design & Programming