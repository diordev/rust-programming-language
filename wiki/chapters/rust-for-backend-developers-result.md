---
title: "Rust for Backend Developers: Result"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, result, chapter]
source_count: 1
---

# Rust for Backend Developers: Result

## Learning Goal

`Result<T, E>`ni recoverable error modeli sifatida tushunish; custom error type, [[std-error-trait|std::error::Error]], combinators, `?`, va ignored `Result` signalini bitta amaliy chiziqqa tushirish.

## Main Ideas

- `Result<T, E>` success value va error value'ni bitta enum ostida saqlaydi.
- String error boshlang'ich nuqta bo'lishi mumkin, lekin custom enum error programmatic handling uchun yaxshiroq.
- [[std-error-trait|std::error::Error]] standard ecosystemdagi error contract'ni beradi; qo'lda impl mumkin, lekin helper crate'lar ko'p ishlatiladi.
- `map` va `and_then` Result compositionni qisqartiradi, lekin `?` ko'pincha linear o'qilishni qaytaradi.
- `?` errorni handle qilmaydi; compatible return type bilan callerga propagate qiladi.
- Ignored `Result` ataylab bo'lsa, `let _ = ...;` bilan explicit discard kerak.

## Concepts

- [[result|Result]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[std-error-trait|std::error::Error]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]
- [[discarded-binding]]

## Examples

```rust
fn square_root(num: f32) -> Result<f32, String> {
    if num < 0.0 {
        Err("Cannot calculate for negative number".to_string())
    } else {
        Ok(num.sqrt())
    }
}
```

```rust
fn read_text_file(file_name: &str) -> Result<String, std::io::Error> {
    let mut file = std::fs::File::open(file_name)?;
    let mut contents = String::new();
    use std::io::Read;
    file.read_to_string(&mut contents)?;
    Ok(contents)
}
```

```rust
let _ = function_that_may_fail();
```

## Exercises

- String error qaytaradigan functionni custom enum error bilan qayta yozing.
- Manual `match`, combinator, va `?` bilan bir xil Result flow'ni yozing.
- Ignored `Result` warningini chaqirib, keyin explicit discard bilan bosing.

## Review Questions

- Qachon custom error enum string error'dan yaxshiroq?
- `?` nega error handling emas, propagation operatori?
- Nega ignored `Result` compiler uchun shubhali signal?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-result]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[result|Result]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[std-error-trait|std::error::Error]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]
- [[discarded-binding]]
