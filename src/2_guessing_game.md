# 2. Guessing Game

## 2.1 Bring Library into Scope

```rust,ignore
use std::io;
```

## 2.2 Mutable V.S. immutable

```rust,ignore
let apples = 5; // immutable
let mut bananas = 5; // mutable
```

By default variables are immutable.

## 2.3 Read Input

```rust,ignore
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
```

One long line is difficult to read, so it’s best to divide it.

`read_line` returns a value, an `io::Result`. The Result types are enums. For Result, the variants are:
- `Ok`: the operation was successful, and inside Ok is the successfully generated value;
- `Err`: the operation failed, and Err contains information about how or why the operation failed.

`expect`:
- if `io::Result` is an `Err` value, expect will cause the program to crash and display the message that you passed as an argument to expect;
- if `io::Result` is an `Ok` value, expect will take the return value that Ok is holding and return just that value to you so you can use it.

## 2.4 Line Print with Placeholder

```rust
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
```

## 2.5 Adding Dependencies

In `Cargo.toml` file, add:

```rust,ignore
rand = "0.8.3"
```

Here, SEMVER is used.

Update cargo:

```bash
cargo update
```

## 2.6 RNG

```rust,ignore
use rand::Rng;

fn main() {
    let secret_number = rand::thread_rng().gen_range(1..101);
    println!("The secret number is: {}", secret_number);
}
```

## 2.7 `gen_range`

gen_range(start..end):

- inclusive on the lower bound
- exclusive on the upper bound
- range `1..101` is equivalent to `1..=100`

## 2.8 Match Expression

`std::cmp::Ordering` enum:
- `Less`
- `Greater`
- `Equal`

A match expression is made up of arms:

```rust,ignore
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

## 2.9 Trim & Parse: Handle Input

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

The `trim` method eliminates `\n` or `\r\n`.

- `parse` method on strings: parses a string into some kind of number
- could easily cause an error
- returns a `Result type`, needs to be handled

## 2.10 Loop and Break

```rust,ignore
    loop {
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                break;
            }
        }
    }
```
