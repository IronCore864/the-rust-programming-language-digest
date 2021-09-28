# 1. Getting Started

## 1.1 Install

`rustup`: a command line tool for managing Rust versions and associated tools.

```bash
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh

# A C compiler is required
# because some common Rust packages depend on C code and will need a C compiler.
# On macOS, you can get a C compiler by running:
xcode-select --install
```

Update:

```bash
$ rustup update
```

Uninstall:

```bash
rustup self uninstall
```

Version check:

```bash
rustc --version
```

## 1.2 Write, Compile and Run

```bash
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
$ touch main.rs
```
**Rust files always end with the *.rs* extension. If you’re using more than one word in your filename, use an underscore to separate them. For example, use *hello_world.rs* rather than*helloworld.rs*.**

Now open the *main.rs* file you just created and enter the code in Listing 1-1.

<span class="filename">Filename: main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

- `fn`
- println! calls a Rust macro. If it called a function instead, it would be entered as println (without the !)
- semicolon

## 1.3 Cargo

Check cargo's version:

```bash
cargo --version
```

Create a project with cargo:

```bash
$ cargo new hello_cargo
$ cd hello_cargo
```

Build and run:

```bash
cargo build
cargo run
```

Cargo also provides a command called **cargo check**. This command quickly checks your code to make sure it compiles but doesn’t produce an executable:

```bash
cargo check
```

When your project is finally ready for release, you can use **cargo build --release** to compile it with optimizations.

This command will create an executable in `target/release` instead of `target/debug`. The optimizations make your Rust code run faster, but turning them on lengthens the time it takes for your program to compile. This is why there are two different profiles: one for development, when you want to rebuild quickly and often, and another for building the final program you’ll give to a user that won’t be rebuilt repeatedly and that will run as fast as possible. **If you're benchmarking your code's running time, be sure to run cargo build --release and benchmark with the executable in target/release.**
