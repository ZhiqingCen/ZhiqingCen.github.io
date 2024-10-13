---
title: Rust Notes
date: 2024-05-01 09:00:00 +1000
categories: [Programming, Rust]
tags: [rust, programming languages, code design]
description: "A deep dive into solving modern programming challenges using the Rust programming language, comparing its strengths and weaknesses to other languages."
---

# Solving Modern Programming Problems with Rust Notes

**Starting on Rust**

- [documentation](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
- In Rust, packages of code are referred to as **crates**
- Cargo expects source files to live inside the src directory
  - the top-level project directory is just for README files, license information, configuration files, and anything else not related to the code
- on CSE machine, add `6991` before `cargo` for any commands (eg. `6991 cargo build`)
  - `$ ls ~cs6991/bin/` - to find all cargo functions
- libraries called crates

```shell
# create new directory and project
$ cargo new [project_name]

# create new project in existing directory
$ cargo init

# build project and creates an executable file
# in target/debug/project_name rather than in current directory
$ cargo build
   Compiling helloworld v0.1.0 (/Users/zhiqingcen/Documents/COMP6991/lab/lab00/helloworld)
    Finished dev [unoptimized + debuginfo] target(s) in 0.85s

# run the executable
$ ./target/debug/[project_name]
Hello, world! # output

# compile the code and run the resultant executable all in one
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/helloworld`
Hello, world!

# command line argument follow by --
$ cargo run -- [arguments]

# checks code to make sure it compiles but doesn’t produce an executable
# much faster than `cargo build` since not producing executable
$ cargo check
    Checking helloworld v0.1.0 (/Users/zhiqingcen/Documents/COMP6991/lab/lab00/helloworld)
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s

# add new library package
$ cargo add [package_name]

# expand macros
$ cargo expand
```

building for release

- release makes code run faster, but longer compile time
- compile code with optimizations
- create an executable in `target/release` instead of `target/debug`

```shell
$ cargo build --release
$ cargo run --release
```

**Cargo.toml**

- in Tom’s Obvious, Minimal Language format (Cargo’s configuration format)
- `[package]` - section heading that indicates that the following statements are configuring a package
- `[dependencies]` - the start of a section for listing any project’s dependencies

```rust
[package]
name = "helloworld"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

### Syntax

- TOCTOU - time of check, time of use
- no `NULL` value in Rust, `Option` is a replacement for `NULL`
- `let` statement
  - create bindings, shadowing, aside: constants
  - default is immutable data, make it mutable when there is a need only
  - define variable or constant
  - `:` - type declaration
    - most of the time rust can figure out type on its own
  - `mut` - variable
    - mutability, changable variable
- constant
  - `CONST MY_CONST: i32 = 42;`

```rust
struct Student {
    name: String,
    zid: u32,
    wam: Option<f64>, // option means can be null
}

CONST MY_CONST: i32 = 42;

fn main() {
  // constant
    let x: u8 = 42;
    // variable
    let mut y: i16 = 42;
    y = 123;

    let mut student = Student { :Student
        name: String::from("Student McStudentFace"),
        zid: 5555555,
        wam: Some(100.0)
    };
    student.wam = Some(50.0);

    // mut has to bind with variable name
    let (mut a, mut b) = (1, 2);

    // shadowing, the second z take over the name, can change type
    // now z reference to the second z
    let z = 42;
    let z = 123;

    // ------------------- //
    // start of scope a
    let z = 42;
    {
      // start of scope b
        let z = 123;
        println!("the value is: {x:?}");
        // end of scope b
    }
    println!("{x}");
    // end of scope a
    // output from command line
    // the value is: 123
    // 42
    // ------------------- //

    // mutate a then change it back to unmutable
    let a;
    {
        let mut b = 42;
        // calculation on b
        b += 30
        b /= 2
        a = b;
    }
}
```

#### Basic Type

- integer
  - fix size
    - sign int
      - `i8`, `i16`, `i32`, `i64`, `i128`
    - unsign int
      - `u8`, `u16`, `u32`, `u64`, `u128`
  - arch
    - `isize`
    - `usize` - used for array indexes
- floating point
  - `f32`, `f64`
- boolean
- character

```rust
struct Student {
    name: String,
    zid: u32,
    wam: Option<f64>,
}

fn create_student(...) -> Student {
    // dependent typing
    assert!(0 <= wam && wam <=100)
}

fn main() {
    let x: i32 = 421213;
    // force a big number into a small size
    // rust will truncate it, eg. 246 in this case
    let y: u8 = x as u8; // not suggested

    // try to turn into u8, if not print message in expect()
    let y: u8 = x.try_into().expect("number is too big");

    let a: bool = false;
    let c: char = 'a';
}
```

#### Compound Type

- array
  - need to specify size of array
  - growable array type `Vec`
  ```rust
  // length = 5, datatype = i32
  let my_array: [i32; 5] = [
      1,
      2,
      3,
      4,
      5,
  ];
  let mut my_vec = vec![1, 2, 3];
  my_vec.push(42);
  ```
- tuple

  ```rust
  let my_tuple: (bool, i32, char) = (true, 42, 'a');
  let student_tuple: (String, u32, Option<f64>) = todo!();
  let one_tuple: (i32) = (5+2, ); // need a comma
  let unit_tuple: () = (); // empty tuple, zero in size

  if unit_tuple == () {
      // unit_tuple is sometimes useful
  }
  ```

#### Struct

- known for product type

```rust
struct Student {
    name: String,
    zid: u32,
    wam: Option<f64>,
}
```

#### Enums

- known for sum type

```rust
enum CarBrand {
    // can attach different arbitrary data type to each enum data
    Toyota(i32),
    Nissan(String),
    Subaru,
}

fn main() {
    let brand: CarBrand = CarBrand::Toyota(42);
    let brand: CarBrand = CarBrand::Nissan(String::from("hello"));
    let brand: CarBrand = CarBrand::Subaru;
}

enum Option<T> {
    None,
    Some(T),
}

type Foo = Option<i32>;
```

#### Expression vs. Statement

- `if` is an expression
- value production
- implications on nesting
- item declarations
- let statement
- expression statement
  - any expression is also a valid statement

```rust
let my_variable = if x == 42 {
    let y = 5;
    y * 2
    // last phrase of a block don't have semicolon at the end
    // then it is the return data
} else {
    50
};
```

#### Functions

- parameters, return type, early-return, expression-return
  - generic type parameters
  - associate type parameters
    - only implement once per type
- private by default

```rust
fn add_5(x: i32) -> i32 {
    let value = x + 5;
    value
}

fn takes_string(x: String) -> String {
    println!("{x}");

    x
}
```

#### If Expression

- branching
- what is a valid condition
- as an expression
- ternary if?

```rust
if my_option.is_some() {
    let value = my_option.unwrap();
}

println!("{expression_value}");

if let Some(value) = my_option {
    println!("The value exists, and it is {value}");
}


let my_result: Result<i32, String> = Err(String::from("oh no!"));
match my_result {
    Ok(number) => {
        println!("Ok! The number is {number}");
    }
    Err(message) => {
        println!("Err! The message is {message}");
    }
}

if true { return }
```

#### Loop

- loop, while, for
- early termination
- loop break value

```rust
for elem in my_array {
    println!("{elem}");
}

while x < 10 {
}

// use loop instead of while true
while true {
}
loop {
}

```

#### Ownership

- eg. string, vec
- every value in Rust has exactly one owner
- transfer of ownership (move)
- drop (vs gc? rc?)
- escape hatch: clone
- ownership and function
  - passing variable as parameter takes ownership of variable
  - to solve this, make function return the variable back

```rust
let my_string = String::from("hello");
// move value from original owner my_string to new owner my_other_string
let my_other_string = my_string;
// correct way to keep both variable
let my_other_string = my_string.clone();
```

#### Copy

- does not follow ownership
- value semantics
- simple scalar types
- tuples? arrays?
  - in rust, empty tuple represents no data
- structs? enums?
- a type to be copy must exist on the stack, there can be non-copy things on the stack
- copy is implicit, clone is explicit

```rust
#[derive(Clone, Copy)]
struct Foo {

}
```

#### Ownership and function

- passing variable as parameter takes ownership of variable
- to solve this, make function return the variable back

```rust
let my_string = String::from("hello");
fn take_string(x: String) -> String {
    x
}
take_string(my_string) // ??? TODO
```

#### Match

- pattern matching
- exhaustiveness
- catch-all

```rust
let my_option: Option<i32> = Some(42);
let my_option: Option<i32> = None;

let x = 42;
let y = 123;

let (x, y) = (y, x);

// match 42 {
//     0 => todo!(),
//     1 => todo!(),
//     2 => todo!(),
//     3 => todo!(),
//     _ => todo!(),
// }

let expression_value = match my_option {
    Some(value) => {
        println!("The value is {value}");

        value
    }
    Some(42) => {
        println!("42!");

        42
    }
    hello => {
        println!("Value = {hello:?}");

        999
    }
};

let my_result: Result<i32, String> = Err(String::from("oh no!"));

match my_result {
    Ok(number) => {
        println!("Ok! The number is {number}");
    }
    Err(message) => {
        println!("Err! The message is {message}");
    }
}
```

#### Optional Values

- **Rust** - `Option<T>`
  - non-magic type
  - no implicit nullability
  - no implicit unwrapping
  - unwrap `None` => unwind
- **C** - `<type> \*`
  - compiler build-in (magic)
  - implicit pointer nullability
  - unwrap on `*` and `->`
  - dereference `Null` => UB
  - robust programs must be defensive~
- **C++ 17** - `std::optional<T>`
  - similar to Rust `Option`
  - non-magic type
  - implicit pointer nullability
  - unwrap `nullopt` => UB
- **Java** - `Optional<T>`
  - similar to rust option
  - non-magic type
  - implicit object nullability
  - unwrap `empty` => unwind
  - `Optional<T>` can be Null
- **Golang** - `* <type>`
  - similar to C
  - compiler build-in (magic)
  - implicit pointer nullability
  - dereference Null => crash
  - using pointers can be painful~
- **Python** - `None`
  - any value in Python can be `None`
  - implicit unwrap everywhere
  - unwrap `None` => unwind
  - life on the edge!

```rust
// What does an "optional value" ever represent?
fn string_to_int(String &str) -> Option<i32> {
    // this function may not produce an output
}

fn int_to_string(int: i32, base: Option<u32>) -> String {
    // this function may not require an input
}

struct Student {
    zid: u32,
    name: String,
    wam: Option<f64>, // this type may not hold some data
}
```

## Collections


Common Collections

- Array
- Vec
- VecDeque
- String
- LinkedList
- HashMap
- BTreeMap
- HashSet
- BTreeSet
- BinaryHeap

- Sequences
  - Vec
  - VecDeque
  - LinkedList
- Maps
  - HashMap
  - BTreeMap
- Sets
  - HashSet
  - BTreeSet
- Misc.
  - BinaryHeap

## Error Handling

- prefer to use `?` for error checking
- sentinel values
  - commonly seen in C, Go, Pascal
- Checked Exceptions
  - commonly seen in Java
  - definition
    - Checked exceptions are the types of exceptions that are checked at compile time.
    - This means the compiler requires the code to either handle these exceptions with a try-catch block or declare them in the method signature using the throws keyword.
  - purpose
    - They are used for scenarios where the programmer can reasonably expect to recover from the error.
    - Checked exceptions are typically used for recoverable conditions and might be caused by events outside the control of the program (e.g., file not found, network errors).
- Unchecked Exceptions
  - commonly seen in Java, C++, C#, Python, Swift, Kotlin, ML family, ...Rust?
    - While Rust does not have unchecked exceptions in the way languages like Java do, it handles error conditions through a combination of Result and Option types for recoverable errors and panics for situations that are considered unrecoverable. This design choice reflects Rust's emphasis on safety, performance, and explicit error handling
  - definition
    - Unchecked exceptions are the types of exceptions that are not checked at compile time. In languages like Java, these include RuntimeException and its subclasses. Unchecked exceptions can occur anywhere in the program, and the compiler does not mandate handling them.
  - purpose
    - They usually indicate programming bugs, such as logic errors or improper use of an API. Unchecked exceptions are used for conditions from which the program cannot reasonably be expected to recover.
- Sum Types
  - commonly seen in Rust, Swift, Haskell, ML family, Java 15 / 17

## Ownership &amp; Borrowing, lifetimes

### Ownership &amp; Borrowing / Reference

- one owner
- responsible for things it own
- it can be move around
- shared 'xor' mutable
  - can be shared or can be mutable, but not both
  - shared borrows are immutable, can have many of shared borrows of a value at the same time
  - exclusive borrow is not shared, can only have 1 at a time

|    T = thread    | T1 read | T1 write | T1 no access |
| :--------------: | :-----: | :------: | :----------: |
|   **T2 read**    |    ✔️    |    ✖️     |      ✔️       |
|   **T2 write**   |    ✖️    |    ✖️     |      ✔️       |
| **T2 no access** |    ✔️    |    ✔️     |      ✔️       |

- `&` operator represents borrowing
  - create a borrow
  - or as a type of a borrow
- shared borrows - `&`
- exclusive borrow - `&mut`
  - only mutable variable can be exclusive borrowed
- borrow type always have a known size

```rust
fn main() {
  let mut image = Image::new(100, 100);
  draw_line(&mut image); // this `&mut` create an exclusive borrow
}

fn draw_line(image: &mut Image) {
  //this `&mut` declare that the variable `image` is of type exclusively borrowed Image
}
```

- `'static` - a borrow that lives forever

### Slice

- `&[T]` - a combination of array and vec
- can be any length, but borrow type always have a known size
- avoid borrow an array or a vec, use slice instead
- cannot change length of slice even with an exclusive borrow
- only combinations which stores its elements continuously in memory can turn into slice

  - eg. `Vec`
  - `Vecdeque` can't be turn into slice

  ```
            start
                          end
  Vecdeque = [1, 2, 3, 4, 5]

  # remove first element
                start
                          end
  Vecdeque = [_, 2, 3, 4, 5]

  # add another element
                start
              end
  Vecdeque = [6, 2, 3, 4, 5]
  # thus incontinuous
  ```

- example

  - `String` is like a vec, mutable
  - `str` is like a slice of it, not mutable

- what types are copy?
- can i make a new copy type?

### Lifetime

- shared borrow exists before program finished
- `'static` - a borrow that lives forever
- lifetime rules on function
  - each place that an input lifetime is left out (a.k.a 'elided') is filled in with its own lifetime
  - if there's exactly one lifetime on all the input references, that lifetime is assigned to every output lifetime

## Documentation, testing, modularity

### Module

```rust
fn main() {
    // not working, foo() is private
    hello::foo();
    // this works, function is public
    let mut example = hello::pub_foo();

    // works, country in struct pub_world is public
    println!(example.pub_world.country);
    // not working, population is private
    println!(example.pub_world.population);
}

mod hello {
    fn foo() {}
    pub fn pub_foo() {}
    struct world {}
    pub struct pub_world {
        pub country: String,
        population: i32,
    }
    mod hi {}
}
```

```rust

struct Student {
    name: String,
}

impl Student {
    // getter
    pub fn name(&self) -> &str {
        &self.name
    }

    // setter
    pub fn name_mut(&mut self, new_name) -> &mut str {
        self.name = new_name
    }
}
```

Package with both a library and a binary

```rust
// Cargo.toml
[package]
name = "mything"
version = "0.0.1"
authors = ["me <me@gmail.com>"]

[lib]
name = "mylib"
path = "src/lib.rs"

[[bin]]
name = "mybin"
path = "src/bin.rs"

// src/lib.rs:
pub fn test() {
    println!("Test");
}

// src/bin.rs:
extern crate mylib; // not needed since Rust edition 2018
use mylib::test;
pub fn main() {
    test();
}
```

### Documentation

```rust
//! this document the whole library
//! initial documentation of library
//! all this code is in lib.rs

/**
 * documentation
 *
 *
 */

/// rust doc is markdown
/// write right before a function
/// # Example
///
/// this is my example
/// can write markdown code block
pub fn my_favourite_number() -> i32 {
    42
}

pub fn foo() {
    //! this is documentation for foo() only
    //!
}
```

- a good convention is to write documentation in the form of a test
  - put the following code in markdown code block as documentation
  ```rust
  # use mylib::add_5;
  assert_eq!(5, add_5(0));
  ```
  - run this with `cargo test --doc`

### Testing

- unit test
  - in the same file as code
  - place at the bottom of the file

```rust
pub fn add_5(x: i32) -> i32 {
    x + 5
}

#[cfg(test)]
mod tests {
    use crate::add_5;

    #[test]
    fn it_works() {
        assert_eq!(5, add_5(0));
    }

    #[test]
    fn test_2() {
        assert_eq!(10, add_5(5));
    }
}
```

- integration test
  - make a test directory `tests`
  - add a file `any_name.rs` and create a main file within

```rust
// import file
fn main() {
    assert_eq!(5, add_5(0));
}
```

## Polymorphism

```rust
/// What restrictions exist on X, Y, Z?
/// 1. Z = X (in case x is smaller)
/// 2. Z = Y (in case y is smaller)
///   => X = Y = Z = T
/// 3. T must be comparable
///   => T is a generic type parameter
///
// fn smallest_two(x: X, y: Y) -> Z {
//     if x < y {
//         x
//     } else {
//         y
//     }
// }

use std::cmp::PartialOrd;
fn main() {
    dbg!(smallest_two::<i32>(100, 42));
    dbg!(smallest_two::<f32>(3.14, 99.9));
    dbg!(smallest_two::<char>('a', 'z'));
    // this also works
    dbg!(smallest_two(100, 42));
    dbg!(smallest_two(3.14, 99.9));
    dbg!(smallest_two('a', 'z'));
}

fn smallest_two<T>(x: T, y: T) -> T
where
    T: PartialOrd,
{
    if x < y {
        x
    } else {
        y
    }
}

fn smallest_n<T>(list: Vec<T>) -> Option<T>
where
    T: PartialOrd,
{
    let mut smallest = list.pop()?;
    while let Some(item) = list.pop() {
        if item < smallest {
            smallest = item;
        }
    }
    Some(smallest)
}

fn smallest_n<T, I>(list: I) -> Option<T>
where
    T: PartialOrd,
    I: IntoIterator<Item = T>,
{
    let mut iter = list.into_iter();
    let mut smallest = iter.next()?;

    for item in iter {
        if item < smallest {
            smallest = item;
        }
    }
    Some(smallest)
}
```

- monomorphization
  - compiler copy and paste to generate functions for different usecase combination of function accecpting multiple types

### Parametric Polymorphism

- allows a single piece of code to be given a "generic" type, using variables in place of actual types, and then instantiated with particular types as needed
- Parametrically polymorphic functions and data types are sometimes called generic functions and generic datatypes, respectively, and they form the basis of generic programming

### Ad hoc Polymorphism

- a kind of polymorphism in which polymorphic functions can be applied to arguments of different types, because a polymorphic function can denote a number of distinct and potentially heterogeneous implementations depending on the type of argument(s) to which it is applied
- given a distinct definition for each type
  - can generally only support a limited number of such distinct types, since a separate implementation has to be provided for each type

### Trait

- similar to interface in Java
- but have extra features that sometimes require user to define type of a parameter

### Trait Object

- trait object is discourage in Rust
- implement the base trait, its auto traits, and any supertraits of the base trait

## Meta-programming

- meta-programming
  - code that generate codes
- meta-programming examples
  - compiler plugins
    - eg. Project lombok (Java)
  - Macro systems
    - eg. C, Rust
  - Runtime reflection
    - eg. Java, C#
  - Embedded DSLs (Domain Specific Language)
    - eg. jOOQ (Java), elisp
    - combinators, eg. Nom

### Macro

- `literal` - literal (something like 'a', or 3, or "hello")
  - the `$metavar:literal` is saying that you're capturing any literal, and naming it `metavar`
- `expr` - expression
- `stmt` - statement
  - similar to `expr`, but allows Rust statements too, like `let` statements
- `ident` - an "identifier", like a variable name
  - ident metavariables Can be followed by anything
- `block` - a "block expression" (curly braces, and their contents)
  - Can be followed by anything
- `ty` - a type
  - Can only be followed by `=>`, `,`, `=`, `|`, `;`, `:`, `>`, `>>`, `[`, `{,` `as`, `where`, or a `block` metavariable

```shell
# expand macro only, not executing it, need to install
$ cargo expand
```

```rust
#[derive(Debug, PartialEq, PartialOrd)]
struct Student {
    zid: u32,
    name: String,
}

macro_rules! max {
    ($a:expr, $b:expr $(,)?) => {
        if $a > $b { $a } else { $b }
    };
}

macro_rules! up_to {
    ($i:ident, $n:expr, $body:block) => {
        for $i in 0..$n $body
    };
}

macro_rules! my_vec {
    ($($elem:expr),* $(,)?) => {
        {
            let mut vec = Vec::new();
            $(
                vec.push($elem);
            )*
            vec
        }
    };
}

macro_rules! cfor {
    // TODO
    () => {};
}

fn main() {
    let x = max!(10, 42);
    println!("{x}");

    let x = max!(
        Student {
            zid: 7654321,
            name: String::from("Irfan"),
        },
        Student {
            zid: 1234567,
            name: String::from("Brian"),
        },
    );

    up_to!(i, 10, {
        println!{"i={i}"};
    });

    let v = my_vec![1, 2, 3];
    println!("{v:?}");

    /*
    cfor! {
        for (let mut i = 0; i < 42; i += 1) {
            println!("i = {i}");
        }
    }
    */

    // sort::test_sort();
}
```

```rust
use paste::paste;

macro_rules! declare_sort {
    ($prefix:ident, $type:ty) => {
        paste! {
            use std::cmp::Ordering;

            fn [<_declare_sort_ $prefix _compare>](x: &$type, y: &$type) -> Ordering {
                if x > y {
                    Ordering::Greater
                } else if x < y {
                    Ordering::Less
                } else {
                    Ordering::Equal
                }
            }

            fn [<$prefix _sort>](slice: &mut [$type]) {
                slice.sort();
            }
        }
    }
}

declare_sort!(int, i32);

pub fn test_sort() {
    let mut xs = [6, 1, 3, 9, 10, 5, 2, 4, 8, 7];
    int_sort(&mut xs);

    println!("Sorted: {xs:?}");
}
```

## Function

- Function pointer `fn(T,U,V,…) -> R`
  - limitations: no environment
  - have no access to environment values
- Closures `|t,u,v,…| <body>`
  - closure traits
    - `FnOnce(T,U,V,…) -> R`
    - `FnMut(T,U,V,…) -> R`
    - `Fn(T,U,V,…) -> R`
  - closure can access their environment

```
FnOnce  >   FnMut  >  Fn   >  fn
most flexible -> least flexible
```

|             | self                         | &amp;mut self                       | &amp;self                                 | Copy                   |
| :---------: | :--------------------------- | :---------------------------------- | :---------------------------------------- | :--------------------- |
|    **T**    | Owned, can only be used once | Can only hold one at a time         | Can have many at a time                   | No ownership semantics |
| **fn type** | **FnOnce**                   | **FnMut**                           | **Fn**                                    | **fn**                 |
|             | - Can only be called once    | - Can only be called once at a time | - Can be called many times simultaneously | - No extra semantics   |
|             |                              |                                     |                                           | - Is Copy!             |

## Concurrency, parallelism

- **Mutex Lock (Mutex)**
  - Mutex Lock is about protecting shared data by serializing access to it.
  - A Mutex, short for mutual exclusion, is a concurrency primitive used to protect shared data. Only one thread can access the data protected by a mutex at a time. If another thread wants to access the data, it must wait until the mutex is unlocked by the current owner. This ensures safe access to shared data by serializing access, which prevents data races.
  - In Rust, the Mutex<T> type provides a way to safely access data across threads. When a thread wants to access the data, it must first lock the mutex, which returns a lock guard. This lock guard provides access to the data and automatically releases the lock when it goes out of scope.
- **Reference Counted (Rc)**
  - Reference Counted (Rc) allows multiple ownership of the same data in single-threaded scenarios.
  - The Rc<T> type is a non-thread-safe reference counting pointer in Rust. It enables multiple owners of the same data, with the data being cleaned up when the last owner goes out of scope. However, because Rc<T> is not thread-safe, it cannot be used directly in concurrent scenarios.
  - Rc<T> is useful for use cases where you want to share data between parts of your program, but the data does not need to be accessed from multiple threads.
- **Atomic Reference Counted (Arc)**
  - Atomic Reference Counted (Arc) extends the concept of reference counting to multi-threaded scenarios by using atomic operations to ensure thread safety.
  - The Arc<T> type is similar to Rc<T> but is designed for use in concurrent scenarios. It is an atomic reference counting pointer, meaning that it can safely be shared across threads. The atomicity guarantees that the reference count is incremented and decremented safely across threads without causing data races.
  - Arc<T> is used when you need to share data between threads or when data needs to live beyond the scope of a single thread. Unlike Rc<T>, Arc<T> uses atomic operations to manage the reference count, making it slightly more expensive than Rc<T> but safe for concurrent use.
    The key differences between these types are their thread safety and use cases:

TODO finish code

```rust
fn main() {
    concurrency_example();
}

fn concurrency_example() {
    let handle_1 = thread::spawn(|| {
        task_1();
    });
    let handle_2 = thread::spawn(|| {
        task_2();
    });

    handle_1.join();
    handle_2.join();
}

fn task_1() {
    loop {
        println("hello");
        sleep(Duration::from_secs(3));
    }
}

// echo user input back out to them, forever
fn task_2() {
    loop {
        let mut line = String::new();
        std::io::stdin().read_line(&mut line);
        println("You entered: {line}");
    }
}
```

## Unsafe, community crates

- rust is split(bisect) into safe and unsafe
- safe rust is a subset of unsafe rust
  - inside unsafe rust, safe rust code still align with its rules
- rust don't have `++` to deal with unsequence modification
- unsafe code will never lead to unsound
- unsafe superpower
  1. dereference raw pointers
  2. call unsafe functions
  3. implement unsafe traits
  4. read/write mutable or external static variables
  5. accessing a field of a union, other than to assign to it (assigning to a union is safe, but read from union is unsafe)

### Foreign Function Interface (FFI)

- assumed to be unsafe
  - need to wrap in `unsafe{}` block
- support C code

## References

- course COMP6991 - Solving Modern Programming Problems with Rust