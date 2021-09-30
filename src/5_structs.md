# 5. Structs

## 5.1 Mutable Struct:

**The entire instance must be mutable; Rust doesn't allow us to mark only certain fields as mutable.**

## 5.2 Field Init Shorthand

The main benefit of using methods instead of functions, in addition to using method syntax and not having to repeat the type of self in every method's signature, is for organization.

Use the same name as the fields:

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

fn build_user(email: String, username: String) -> User {
    User {
        email,      // field init shorthand
        username,   // field init shorthand
        active: true,
        sign_in_count: 1,
    }
}

fn main() {
    let user1 = build_user(
        String::from("someone@example.com"),
        String::from("someusername123"),
    );
}
```
The `String` type rather than the `&str` string slice type is used in the example above.

This is a deliberate choice because we want instances of this struct to own all of its data and for that data to be valid for as long as the entire struct is valid.

## 5.3 Struct Update Syntax

```rust,ignore
fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@example.com"),
        username: String::from("anotherusername567"),
        ..user1     // struct update syntax
    };
}
```

## 5.4 Tuple Structs

Tuple structs are useful when you want to give the whole tuple a name and make the tuple be a different type from other tuples, and naming each field as in a regular struct would be verbose or redundant.

Example:

```rust
fn main() {
    struct Color(i32, i32, i32);
    struct Point(i32, i32, i32);

    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

## 5.5 Struct as param

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        area(&rect1)
    );
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

## 5.6 Adding Useful Functionality with Derived Traits

In the `println!`, by default, the curly brackets tell `println!` to use formatting known as `Display`.

In format strings you may be able to use `{:?}` (or `{:#?}` for **pretty-print** instead, using the `Debug` trait.

We have to explicitly opt in to make that functionality available for our struct.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {:#?}", rect1);
}
```

## 5.7 Method

Methods are different from functions in that they're defined within the context of a struct, and their first parameter is always self, which represents the instance of the struct the method is being called on.

The main benefit of using methods instead of functions, in addition to using method syntax and not having to repeat the type of self in every method's signature, is for organization.

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        // here &self is used instead of self,
        // beacuse we only want to read data, instead of taking ownership
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

The `impl` block defines on which struct the method is implemented.

Each struct is allowed to have multiple impl blocks.

Methods can take ownership of `self`, borrow self immutably (in the code above), or borrow self mutably.

If we wanted to change the instance that we've called the method on as part of what the method does, we'd use `&mut self` as the first parameter.

## 5.8 Automatic Referencing and Dereferencing

When you call a method with `object.something()`, Rust automatically adds in `&`, `&mut`, or `*` so object matches the signature of the method.

```rust,ignore
p1.distance(&p2);
(&p1).distance(&p2);
```

They are the same.

## 5.9 Associated Functions

Functions within impl blocks that don't take self as a parameter are called associated functions because they're associated with the struct.

They're still functions, not methods, because they don't have an instance of the struct to work with.

Associated functions are often used for constructors that will return a new instance of the struct. 

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);
}
```