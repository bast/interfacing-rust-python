class: center, middle

# Title

## Radovan Bast [@\_\_radovan](https://twitter.com/__radovan)

Nordic e-Infrastructure Collaboration/
UiT The Arctic University of Norway

---

## Plan for today

- History of Rust
- Why Rust?
- Why Rust and Python?
- Tools: [PyO3](https://pyo3.rs/) and [Maturin](https://github.com/PyO3/maturin)
- Examples
- Deploying to PyPI with GitHub Actions: make it pip installable


### Examples

- [Nearest distances for a set of points](https://github.com/bast/pyo3-example-distances)
    - Motivation: simple data structures, parallelization
- [Simple bank account](https://github.com/bast/pyo3-example-accounts)
    - Motivation: show how to deal with objects across language boundaries

---

## History of Rust

- Originally designed by Graydon Hoare at Mozilla Research, with contributions from Dave Herman, Brendan Eich, and others.
- Mozilla began sponsoring the project in 2009.
- Announced 2010: http://venge.net/graydon/talks/intro-talk-2.pdf
- Rust 1.0, the first stable release, released in 2015.
- Since 2011 `rustc` is compiled using `rustc`.
- `rustc` uses LLVM as its back end.
- The History of Rust: https://www.youtube.com/watch?v=79PSagCD_AY
- Firefox (components Servo and Quantum) written in Rust.
- Ad block engine in [Brave](https://brave.com/).
- `exa`, `ripgrep`, `bat`, and `fd` written in Rust (you really want these tools!).
- Great overview of tools written in Rust: https://www.wezm.net/v2/posts/2020/100-rust-binaries/
- [Discord](https://discord.com/) uses Rust for some of its back end.

---

## Why Rust

- "A language empowering everyone to build reliable and efficient software." (https://www.rust-lang.org)
- Fast but also safe
- Zero cost abstractions
- Type system
- Compiler catches most errors (if it compiles, it often just works)
- Compiler provides helpful error messages
- Memory safety
- Thread safety
- Private and immutable by default
- Great tooling (testing, documentation, auto-formatter, dependency management, package registry, no makefiles needed)
- Community
- Most-loved programming language in the 2016, 2017, 2018, 2019, and 2020
  [Stack Overflow annual surveys](https://insights.stackoverflow.com/survey/)

---

## Resources and reading

### Books

- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [The Rust Programming Language](https://doc.rust-lang.org/book/)
- [The Rustonomicon](https://doc.rust-lang.org/nomicon/)
- [Rust Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/)
- [Rust in Action](http://www.rustinaction.com/)
- [Programming Rust](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/)


### Blogs/articles

- [Why try Rust for scientific computing?](https://erambler.co.uk/blog/why-give-rust-a-try/)
- [Why scientists are turning to Rust](https://www.nature.com/articles/d41586-020-03382-2)


### Mixing Rust with other languages

- [The Rust FFI Omnibus](http://jakegoulding.com/rust-ffi-omnibus/)

---

## Memory model

- No explicit allocation and deallocation.
- No garbage collector either.
- Each value in Rust has an owner and there can only be one owner at a time.
- When the owner goes out of scope, the value is dropped.
- Rust knows the size of all stack allocations at compile time.

This does not compile:
```rust
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

- Rust's memory is managed a bit like money: one owner, it can be borrowed.
- We can borrow references: https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html#references-and-borrowing
- At any given time, you can have either one mutable reference or any number of immutable references.

---

## Installing Rust

Preferred way to install Rust for most developers: https://www.rust-lang.org/tools/install

Verify your installations of `cargo` and `rustc`:
```
$ cargo --version
cargo 1.47.0 (f3c7e066a 2020-08-28)

$ rustc --version
rustc 1.47.0 (18bf6b4f0 2020-10-07)
```

---

## Hands-on demo

In this demo we will approximate pi by generating random points and computing
their distance to origin:<sup>[1](#footnote1)</sup>

![random points](img/pi_Monte-Carlo.gif)

Tasks/discussion points:
- Show how to bootstrap a project with `cargo new`
- Step out of the newly bootstrapped project
- Clone the code (this repository)
- Browse and discuss the sources
- Compile the sources with `cargo check`
- Compare `cargo check` and `cargo build`
- Discuss `cargo run`
- Experiment with `cargo fmt`
- Run the tests with `cargo test`
- Generate optimized version with `cargo build --release`
- Generate the documentation with `cargo doc`
- Discuss `cargo publish` and https://crates.io (the Rust package registry)
