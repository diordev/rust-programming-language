---
title: "Immutability"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, variables]
source_count: 1
---

# Immutability

## Short Definition

Immutability binding value'ni reassignmentdan himoyalashidir.

## Why It Matters

Rust variables default immutable, bu accidental mutationni kamaytiradi.

## Mental Model

`let x = 5;` dan keyin `x`ni qayta assign qilish mumkin emas; kerak bo'lsa `mut` explicit yoziladi.

## Syntax and Examples

```rust
let x = 5;
// x = 6; // error
```

## Common Mistakes

- Rust variables default mutable deb o'ylash.
- [[shadowing]] va `mut`ni bir xil mexanizm deb tushunish.

## Related Concepts

- [[variables-and-mutability|variables and mutability]]
- [[constants]]
- [[e0384-cannot-assign-twice|E0384 cannot assign twice]]

## Sources

- [[3-1-variables-and-mutability-the-rust-programming-language]]
