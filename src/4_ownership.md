# 4. Ownership

All programs have to manage the way they use a computer's memory while running. Some languages have garbage collection; in other languages, the programmer must explicitly allocate and free the memory. Rust uses a third approach: memory is managed through a system of **ownership** with a set of rules that the compiler checks at compile time. None of the ownership features slow down your program while it's running.

A word on GC: Garbage collection frees the programmer from manually deallocating memory. This eliminates or reduces some categories of errors: dangling pointers, double free bugs, certain kinds of memory leaks, etc. The disadvantages is that, it consumes computing resources in deciding which memory to free, even though the programmer may have already known this information.

## 4.1 Stack & Heap

All data stored on the stack must have a known, fixed size. Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

Pushing to the stack is faster than allocating on the heap. Accessing data in the heap is slower than accessing data on the stack. Allocating a large amount of space on the heap can also take time.

When your code calls a function, the values passed into the function (including, potentially, pointers to data on the heap) and the function's local variables get pushed onto the stack. When the function is over, those values get popped off the stack.

## 4.2 Ownership Rules

Keeping track of what parts of code are using what data on the heap, minimizing the amount of duplicate data on the heap, and cleaning up unused data on the heap so you don't run out of space are all problems that ownership addresses. Once you understand ownership, you won't need to think about the stack and the heap very often, but knowing that managing heap data is why ownership exists can help explain why it works the way it does.

- Each value in Rust has a variable that's called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

```rust
fn main() {
    {                      // s is not valid here, it’s not yet declared
        let s = "hello";   // s is valid from this point forward
        // do stuff with s
    }                      // this scope is now over, and s is no longer valid
}
```

## 4.3 The String Type (on the heap)

The data types mentioned in Chapter 3 are all in the stack.

The `String` type is on the heap.

## 4.4 Move and Clone

```rust,ignore
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;                // s1 "moved" to s2, doesn't do "deepcopy", but rather a shallow copy.
    println!("{}, world!", s1); // s1 can't be borrowed because it's already moved to s2
}
```

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
}
```

`clone()` is like deepcopy.

For stack only data, no need to clone:

```rust
fn main() {
    let x = 5;
    let y = x;  // clone; x isn't moved to y

    println!("x = {}, y = {}", x, y);  // x still available
}
```

If a type implements the Copy trait, an older variable is still usable after assignment. Rust won’t let us annotate a type with the Copy trait if the type, or any of its parts, has implemented the Drop trait.

Any group of simple scalar values can implement `Copy`, and nothing that requires allocation or is some form of resource can implement Copy.

Here are some of the types that implement `Copy`:

- All the integer types, such as u32.
- The Boolean type, bool, with values true and false.
- All the floating point types, such as f64.
- The character type, char.
- Tuples, if they only contain types that also implement Copy. For example, (i32, i32) implements Copy, but (i32, String) does not.

## 4.5 Functions and Ownership

The semantics for passing a value to a function are similar to those for assigning a value to a variable. Passing a variable to a function will move or copy, just as assignment does.

Returning values can also transfer ownership. 

## 4.6 References and Borrowing

`&` and `*` (dereferencing), like in other languages.

Just as variables are immutable by default, so are references. We’re not allowed to modify something we have a reference to.

**Mutable reference:**

```rust
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Only one mutable reference to a particular piece of data in a particular scope. This code will fail:

```rust,ignore
fn main() {
    let mut s = String::from("hello");
    let r1 = &mut s;
    let r2 = &mut s;
    println!("{}, {}", r1, r2);
}
```

No combining mutable and immutable references.

## 4.7 Dangling References

```rust,ignore
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Danger!
```

## 4.8 Rules of References

- At any given time, you can have either one mutable reference or any number of immutable references.
- References must always be valid.

## 4.9 The Slice Type

Slice also doesn't have ownership. Type: `&str`.

We can create slices using a range within brackets by specifying [starting_index..ending_index].

With Rust’s .. range syntax, if you want to start at index zero, you can drop the value before the two periods. In other words, these are equal:

```rust
fn main() {
    let s = String::from("hello");

    let slice = &s[0..2];
    let slice = &s[0..=1];
    let slice = &s[..2];
}
```

You can also drop both values to take a slice of the entire string. So these are equal:

```rust
fn main() {
    let s = String::from("hello");
    let len = s.len();
    let slice = &s[0..len];
    let slice = &s[..];
}
```

**String literals are slices.**
