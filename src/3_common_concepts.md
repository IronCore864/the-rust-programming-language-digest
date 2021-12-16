# 3. Common Concepts

## 3.1. Variables and Mutability

## 3.1.1 Mutability

By default, variables are immutable.

Error:

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
}
```

Make it mutable by adding mut in front of the variable name:

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

### 3.1.2 Differences Between Variables and Constants

- constants can't be mut
- declaration: constants: `const` keyword
- constants can be declared in any scope, including the global scope
- constants may be set only to a constant expression, not the result of a function call or any other value that could only be computed at runtime

Here's an example of a constant declaration where the constant's name is THREE_HOURS_IN_SECONDS and its value is set to the result of multiplying 60 (the number of seconds in a minute) by 60 (the number of minutes in an hour) by 3 (the number of hours we want to count in this program):

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

### 3.1.3 Shadowing

You can declare a new variable with the same name as a previous variable. The first variable is shadowed by the second.

```rust
fn main() {
    let x = 5;
    let x = x + 1;
    let x = x * 2;
    println!("The value of x is: {}", x);
}
```

Shadowing V.S. mut:

- reassign to this variable without using the let keyword will cause an error
- we're effectively creating a new variable when we use the let keyword again, we can change the type of the value but reuse the same name. Mut can't change type.

## 3.2. Data Types

Rust is a statically typed language, which means that it must know the types of all variables at compile time.

The compiler can usually infer what type we want to use based on the value and how we use it. In cases when many types are possible, we must add a type annotation.

### 3.2.1 Scalar Types

**Integer**

| Length  | Signed | Unsigned |
|---------|--------|----------|
| 8-bit   | i8     | u8       |
| 16-bit  | i16    | u16      |
| 32-bit  | i32    | u32      |
| 64-bit  | i64    | u64      |
| 128-bit | i128   | u128     |
| arch    | isize  | usize    |

Integer types **default to i32**.

Integer literal: for example, 1000. Number literals can also use _ as a visual separator to make the number easier to read, such as 1_000, which will have the same value as if you had specified 1000.

**Floating points**: f32 and f64; the default type is f64 because on modern CPUs it's roughly the same speed as f32 but is capable of more precision.

**Boolean** :true/false

**Character**: four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII.

### 3.2.2 Compound Types

**Tuple**

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
```

Tuples have a fixed length: once declared, they cannot grow or shrink in size.

Destructure:

```rust
fn main() {
    let tup = (500, 6.4, 1);
    let (x, y, z) = tup;
    println!("The value of y is: {}", y);
}
```

**Array**

```rust
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5];
```

## 3.3. Functions

### 3.3.1 Parameters

```rust
fn main() {
    another_function(5, 6);
}

fn another_function(x: i32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

### 3.3.2 Statements and Expressions

Function bodies are made up of a series of statements optionally ending in an expression.

Statements do not return values. Expressions evaluate to something.

Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, which will then not return a value.

### 3.3.3 Return Value

Declare their type after an arrow (->).

In Rust, the return value of the function is synonymous with the value of the final expression in the block of the body of a function.

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```


## 3.4. Comments

In Rust, the idiomatic comment style starts a comment with two slashes `//`, and the comment continues until the end of the line. 

Comments can also be placed at the end of lines containing code, but you'll more often see them used with the comment on a separate line above the code it's annotating.

For comments that extend beyond a single line, you'll need to include `//` on each line.

## 3.5. Control Flow

### 3.5.1 `if` / `else if`

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

### 3.5.2 Use `if` in `let` Statement

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {}", number);
}
```

Both the if arm and the else arm should be the same type.

### 3.5.3 `loop`, `while`

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

Return Value from `loop`:

```rust
fn main() {
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    };
    println!("The result is {}", result);
}
```

Conditional loop with `while`:

```rust
fn main() {
    let mut number = 3;
    while number != 0 {
        println!("{}!", number);
        number -= 1;
    }
    println!("LIFTOFF!!!");
}
```

### 3.5.4 Loop through a Collection

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```
The use of `iter()`.

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```

`rev` to reverse the range.
