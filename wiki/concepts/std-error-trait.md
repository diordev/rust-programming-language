---
title: "std::error::Error"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-18
tags: [rust, error-handling, traits]
source_count: 2
---

# std::error::Error

## Short Definition

`std::error::Error` Rust standard library'dagi error typelar uchun umumiy trait contract.

## Why It Matters

`Result<T, E>` har qanday `E` bilan ishlaydi, lekin ecosystem error typelarini bir xil interface ostida ko'rishni xohlaydi. `std::error::Error` shu unify qiluvchi qatlam: `Display`, `Debug`, va kerak bo'lsa source-chain.

## Mental Model

Bu trait "men error type'iman" degan marker emas, balki formatting va chaining contracti. Amalda custom error type ko'pincha:

- `Debug`
- `Display`
- `std::error::Error`

uchligini beradi.

Source'dagi muhim caveat: qo'lda impl yozish mumkin, lekin real Rust ecosystem'da buni helper crate yoki macro bilan yengillashtirish keng tarqalgan. Amaliy defaultlar: domain/library tarafda [[wiki/crates/thiserror|thiserror]], app-level erased boundary'da esa ko'pincha [[wiki/crates/anyhow|anyhow]].

## Syntax and Examples

```rust
#[derive(Debug)]
enum NameParseError {
    EmptyString,
    NoMiddleName,
}

impl std::fmt::Display for NameParseError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        match self {
            NameParseError::EmptyString => write!(f, "Attempt to parse empty string"),
            NameParseError::NoMiddleName => write!(f, "No middle name found"),
        }
    }
}

impl std::error::Error for NameParseError {}
```

## Common Mistakes

- `Result<T, E>` ishlashi uchun `E` har doim `Error` bo'lishi shart deb o'ylash.
- `Error` impl qilsam bo'ldi, `Display` kerak emas deb o'ylash.
- Qo'lda impl yozishni har doim default yo'l deb qabul qilish.
- `Error` trait errorni o'zi handle qiladi deb o'ylash.

## Related Concepts

- [[result|Result]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[traits]]
- [[display-formatting|Display]]
- [[debug-trait|Debug trait]]
- [[box-dyn-error|Box<dyn Error>]]
- [[question-mark-operator|question mark operator]]
- [[custom-error-enum]]
- [[wiki/crates/thiserror]]

## Sources

- [[wiki/sources/rust-for-backend-developers-result]]
- [[wiki/sources/rust-for-backend-developers-error-handling]]
