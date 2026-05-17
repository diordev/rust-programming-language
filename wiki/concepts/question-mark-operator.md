---
title: "Question Mark Operator"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-13
tags: [rust, error-handling, result, option]
source_count: 2
---

# Question Mark Operator

## Short Definition

`?` operator `Result` yoki `Option` value'ni ochadi; success/some bo'lsa inner value'ni beradi, error/none bo'lsa current functiondan early return qiladi.

## Why It Matters

Rustda error propagation juda ko'p uchraydi. `?` manual `match` va `return Err(e)` boilerplate'ini qisqartirib, happy pathni ko'rinarli qiladi.

Appendix B bu syntax'ni operator jadvalida `expr?` ko'rinishida reference qiladi.

## Mental Model

`expr?`ni "agar yaxshi bo'lsa ichidagi value'ni ol, yomon bo'lsa shu functiondan chiqib ket" deb o'qing.

`Result` uchun:

- `Ok(value)` -> `value`
- `Err(error)` -> `return Err(converted_error)`

`Option` uchun:

- `Some(value)` -> `value`
- `None` -> `return None`

## Syntax and Examples

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

`Option` bilan:

```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}
```

`main` ham compatible return type bilan `?` ishlata oladi:

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;
    Ok(())
}
```

## Common Mistakes

- `()` qaytaradigan functionda `Result` ustida `?` ishlatish.
- `Result` va `Option`ni `?` orqali avtomatik bir-biriga convert bo'ladi deb o'ylash.
- `?` errorni handle qiladi deb o'ylash; u errorni propagate qiladi.
- `?`dan keyin `Ok(...)` bilan success value'ni o'rash kerakligini unutish.

## Related Concepts

- [[error-propagation|error propagation]]
- [[result|Result<T, E>]]
- [[option|Option]]
- [[from-trait|From trait]]
- [[main-function|main function]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
- [[rust-operators-and-symbols]]

## Sources

- [[9-2-recoverable-errors-with-result]]
- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
