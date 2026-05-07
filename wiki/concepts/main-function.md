---
title: "main Function"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, functions]
source_count: 3
---

# main Function

## Short Definition

`main` executable Rust programning entry point functioni.

## Why It Matters

Binary crate ishga tushganda control `fn main()`dan boshlanadi.

## Mental Model

`main` programning boshlanish nuqtasi; undan chaqirilgan functionlar actual ishni bajaradi. Default `fn main()` return type'i `()`, lekin `main` `Result<(), E>` ham qaytarishi mumkin.

## Syntax and Examples

```rust
fn main() {
    println!("Hello, world!");
}
```

`?` ishlatadigan `main`:

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

`Ok(())` successful exit code `0`ga, `Err` esa nonzero exit statusga olib keladi.

## Common Mistakes

- `main` nomini yoki signature'ni noto'g'ri yozish.
- `main.rs` file bilan `main` functionni aralashtirish.
- `()` qaytaradigan `main` ichida `Result` ustida `?` ishlatishga urinish.
- `main -> Result<(), E>`da oxirida `Ok(())` qaytarishni unutish.

## Related Concepts

- [[functions]]
- [[println-macro|println! macro]]
- [[compiler]]
- [[question-mark-operator|question mark operator]]
- [[box-dyn-error|Box<dyn Error>]]
- [[result|Result<T, E>]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]

## Sources

- [[1-2-hello-world-the-rust-programming-language]]
- [[1-getting-started|1. Getting Started]]
- [[9-2-recoverable-errors-with-result-the-rust-programming-language]]
