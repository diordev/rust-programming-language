---
title: "Rust for Backend Developers: Tuples"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, tuples, chapter]
source_count: 1
---

# Rust for Backend Developers: Tuples

## Learning Goal

Tuple'ni fixed-size heterogeneous value sifatida va multiple return values uchun ishlatishni tushunish.

## Main Ideas

- Tuple elementlari turli type bo'lishi mumkin.
- Access `.0`, `.1` yoki destructuring bilan.
- Function return value sifatida tuple juda qulay.
- Slice parameter bilan array va vector uchun bitta function ishlatish mumkin.

## Concepts

- [[tuple]]
- [[functions]]
- [[slices]]
- [[vector|Vec<T>]]

## Examples

```rust
let employee = ("John Doe", 1980, true);
let (name, birth_year, active) = employee;
```

## Exercises

- Functiondan ikki qiymat qaytaring.
- Tuple va structni bitta domain misolda solishtiring.

## Review Questions

- Tuple qachon yetarli, qachon struct kerak?
- `&[T]` parameter tuple example'ida qanday foyda berdi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-tuples]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[tuple]]
- [[slices]]
