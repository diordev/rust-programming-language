---
title: "Rust for Backend Developers: First Look"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, beginner, chapter]
source_count: 1
---

# Rust for Backend Developers: First Look

## Learning Goal

Minimal Rust program syntaxini va `rustc` bilan single-file compile/run workflowini tushunish.

## Main Ideas

- `fn main()` executable Rust program entry pointi.
- Rust C-like syntaxdan foydalanadi: braces va semicolonlar ko'p ishlatiladi.
- `println!` macro terminal output beradi.
- `rustc main.rs` Cargo ishlatmasdan binary yaratadi.

## Concepts

- [[main-function]]
- [[println-macro|println! macro]]
- [[rustc]]
- [[semicolon]]
- [[hello-world]]

## Examples

```rust
fn main() {
    println!("Hello world!");
}
```

```shell
rustc main.rs
./main
```

## Exercises

- `main.rs` yaratib, `rustc` bilan compile qiling.
- `println!`ni `println` qilib ko'rib, compiler xatosini o'qing.

## Review Questions

- `main` function nima uchun kerak?
- Nega `println!` oxirida `!` bor?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-first-look]]
- [[hello-world]]
- [[rustc]]

