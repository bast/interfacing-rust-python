class: center, middle

# Interfacing Rust and Python

<img src="img/python.jpg" style="width: 100%;"/>

---

class: center, middle

# Interfacing Rust and Python

## Radovan Bast [@\_\_radovan](https://twitter.com/__radovan)

Nordic e-Infrastructure Collaboration/
UiT The Arctic University of Norway

---

class: left, middle, inverse

# .strike[How to] interface Rust and Python

# How I

## There are many ways, interested in learning how to improve things

---

## About me

.left-column40[
<img src="img/avatar.jpeg" style="width: 70%;"/>
]

.right-column60[
- Theoretical chemist turned research software engineer.
- I write research software and teach programming to researchers and lead the
  [CodeRefinery project](https://coderefinery.org).
- Developing libraries for computational chemistry and computational geometry
  (used in oceanography).
]

---

## Plan for today

- History of Rust
- Why Rust?
- Why Rust and Python?
- Tools: [PyO3](https://pyo3.rs/) and [Maturin](https://github.com/PyO3/maturin)
- Examples (links below)
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
- Since 2021: [Rust Foundation](https://foundation.rust-lang.org/), independent
  non-profit organization to steward the Rust programming language and
  ecosystem


### Great every-day tools

- `exa`, `ripgrep`, `bat`, and `fd` written in Rust (you really want these tools!).
- https://www.wezm.net/v2/posts/2020/100-rust-binaries/

---

class: center, middle, inverse

# Why Rust?

---

class: center, middle

.quote["There are only two kinds of languages: the ones people complain about and the ones nobody uses."]

Bjarne Stroustrup

---

## Why [Rust](https://www.rust-lang.org/)? (1/2)

- First stable release: 2015
- Most-loved programming language in the 2016, 2017, 2018, 2019, and 2020
  [Stack Overflow annual surveys](https://insights.stackoverflow.com/survey/)

- .emph[Fast but also safe]

- Memory safety

- Thread safety

- Type system and type safety

---

## Why [Rust](https://www.rust-lang.org/)? (2/2)

- Zero cost abstractions

- Compiler catches most errors (if it compiles, it often just works)

- Compiler provides helpful error messages

- Private and immutable by default

- .emph[Great tooling] (testing, documentation, auto-formatter, dependency management, package registry, no makefiles needed)

---

## Why [Rust](https://www.rust-lang.org/)? (FAQ)

.quote["But is it as fast as Fortran/C/C++?"]
- **Yes!**

.quote["How about math/scientific libraries?"]
- **No problem**. It has very good inter-op with C (and Python) so you can link to any C interface.

.quote["Do we need to rewrite our Fortran code now?"]
- **No**. Rust can interface to Fortran via `iso_c_binding`.

.quote["How about shared-memory parallelization?"]
- **Easier**. And thread safe.

.quote["How about MPI?"]
- A **bit too early** for this. I would do the MPI layer with Python/C/C++/Fortran and
  the intra-node with Rust.

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

class: center, middle, inverse

# Why Rust and Python?

---

## Why Rust and Python?

### Take the best of both worlds

- Python has a huge ecosystem and following
- Many people are familiar with `pip install`
- Speed up a bottleneck in the Python code
- Prototype an idea in Python before writing it in Rust
- "Bullet-proofing" a prototype by porting to rust (making it typesafe)


### Often we do not start from scratch

- .emph[Connect existing codes]

---

## How: tools

- PyO3 (Pythonium Trioxide): Rust bindings for the Python interpreter
  - https://pyo3.rs/
  - https://github.com/PyO3/pyo3
  - Very helpful community
- [Maturin](https://github.com/PyO3/maturin): build and publish crates with
  pyo3


### Alternatives

- [cffi](https://cffi.readthedocs.io/) on Python side and Rust just talks to C
  interface
- Using [ctypes](https://docs.python.org/3/library/ctypes.html)
  ([examples](http://jakegoulding.com/rust-ffi-omnibus/))
- ... ?

---

## Examples

- [Nearest distances for a set of points](https://github.com/bast/pyo3-example-distances)
    - Motivation: simple data structures, parallelization
- [Simple bank account](https://github.com/bast/pyo3-example-accounts)
    - Motivation: show how to deal with objects across language boundaries

## We do this as demo/ hands-on

---

class: center, middle, inverse

# Deploying to PyPI with GitHub Actions

## Make it pip installable

---

## Deploying to PyPI with GitHub Actions

- Make it pip installable
- [Example workflow](https://github.com/bast/pyo3-example-accounts/blob/main/.github/workflows/package.yml)

```
$ pip install -i https://test.pypi.org/simple/ accounts
```

Then this should work:
```python
from accounts import Account

account1 = Account(100.0)
account2 = Account(100.0)

account1.deposit(50.0)

account2.withdraw(25.0)

print("balance of account 1:", account1.get_balance())
print("balance of account 2:", account2.get_balance())
```
