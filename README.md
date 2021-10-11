# Slides and material for "Interfacing Rust and Python"

- [Slides](https://cicero.xyz/v3/remark/0.14.0/github.com/bast/interfacing-rust-python/main/talk.md/)


## Based on

- https://github.com/dev-cafe/rust-demo


## Example repositories

- https://github.com/bast/pyo3-example-distances
- https://github.com/bast/pyo3-example-accounts


## Abstract

This presentation and demo focuses on why interfacing Rust and Python can be
interesting and how it can be done.  The talk starts with a short history of
the Rust language and motivates advantages of Rust and Python and of combining
compiled and dynamic languages. The tools PyO3 and Maturin are demonstrated
using two examples where we first show how to transmit simple data structures
across the language interface and later we move on to parallelization aspects
and user-defined data structures. Finally an example is given for how packaging
and deployment of a pip-installable Rust library to PyPI can be automated using
GitHub Actions.
