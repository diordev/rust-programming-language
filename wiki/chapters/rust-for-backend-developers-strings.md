---
title: "Rust for Backend Developers: Strings"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, strings, chapter]
source_count: 1
---

# Rust for Backend Developers: Strings

## Learning Goal

`&str` va `String` orasidagi ownership farqini, UTF-8 modelini, va string creation/append/format workflows'ini tushunish.

## Main Ideas

- `&str` borrowed UTF-8 slice.
- `String` owned heap buffer.
- `String::from`, `String::new`, `.to_string()`, `.as_str()`, `push`, `push_str`, va `format!` amaliy minimum.

## Concepts

- [[string-type|String]]
- [[string-slice|String slice]]
- [[string-literal]]
- [[utf-8|UTF-8]]
- [[format-macro|format! macro]]

## Examples

```rust
let slice = "text";
let owned = String::from(slice);
let borrowed = owned.as_str();
```

## Exercises

- Bir xil textni `&str` va `String`da ifodalang.
- `String::new()` bilan yig'iladigan buffer yozing.

## Review Questions

- `&str` qachon yetarli, qachon `String` kerak?
- `format!` bilan `println!` orasidagi asosiy farq nima?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-strings]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[string-type|String]]
- [[string-slice|String slice]]
