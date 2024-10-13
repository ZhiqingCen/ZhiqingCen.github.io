---
title: Iterator Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_to access the elements of an aggregate object sequentially without exposing its underlying implementation_** - GoF

- Benefits
  - It supports **variations** in the **traversal** of a `collection`
  - It **simplifies** the `interface` to the `collection`
- Usage
  - When you want to **access** a `collection` of objects **without exposing its internal representation**
  - When there are **multiple traversals** of objects need to be supported in the `collection`

In collection framework, we are now using Iterator that is preferred over Enumeration

- java.util.Iterator `interface` uses Iterator Design Pattern

## Implementation

1. Create a `Iterator` interface

```java
public interface Iterator {
    public boolean hasNext();
    public Object next();
}
```

2. Create a `Container` interface

```java
public interface Container {
    public Iterator getIterator();
}
```

3. Create a `NameRepository` class that will implement `Container` interface, with an inner class `NameIterator`

```java
public class NameRepository implements Container {
    public String names[] = {"Robert" , "John" ,"Julie" , "Lora"};

    @Override
    public Iterator getIterator() {
        return new NameIterator();
    }

    private class NameIterator implements Iterator {
        int index;

        @Override
        public boolean hasNext() {
            if(index < names.length) { return true; }
            return false;
        }

        @Override
        public Object next() {
            if(this.hasNext()) { return names[index++]; }
            return null;
        }
    }
}
```

4. Create demo IteratorPatternDemo class

```java
public class IteratorPatternDemo {
    public static void main(String[] args) {
        NameRepository namesRepository = new NameRepository();

        for(Iterator iter = namesRepository.getIterator(); iter.hasNext(); ) {
            String name = (String)iter.next();
            System.out.println("Name : " + name);
        }
    }
}
```

## Notes

- The intent of the Iterator design pattern is to: "Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation." [GoF]
- Exposing representation details of an aggregate breaks its encapsulation. Encapsulate the access and traversal of an aggregate in a separate Iterator object.
- Clients request an Iterator object from an aggregate (say by calling createIterator()) and use it to access and traverse the aggregate.
- Define an interface for accessing and traversing the elements of an aggregate object (next(), hasNext()).
- Define classes (Iterator1,...) that implement the Iterator interface
- An iterator is usually implemented as inner class of an aggregate class. This enables the iterator to access the internal data structures of the aggregate.
- New access and traversal operations can be added by defining new iterators. For example, traversing back-to-front: previous(), hasPrevious().
- An aggregate provides an interface for creating an iterator (createIterator()).
- Clients can use different Iterator objects to access and traverse an aggregate object in different ways.
- Multiple traversals can be in progress on the same aggregate object (simultaneous traversals). However, need to consider concurrent usage issues!
- The Java Collections Framework provides
  - a general purpose iterator
    - `next()`, `hasNext()`, `remove()`
  - an extended listIterator
    - `next()`, `hasNext()`, `previous()`, `hasPrevious()`, `remove()`, ....
