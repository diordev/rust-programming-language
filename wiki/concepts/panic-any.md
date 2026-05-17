---
title: "panic_any"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, panic, runtime]
source_count: 1
---

# panic_any

## Short Definition

`std::panic::panic_any` istalgan `Any + Send + 'static` payload bilan panic qilish imkonini beradi.

## Why It Matters

Panic payload'ini string bilan cheklamasdan structured value tashish mumkin.

## Mental Model

Bu capability mavjud, lekin normal error handling uchun emas. `catch_unwind` bilan ushlansa, payload keyin downcast qilinadi.

## Syntax and Examples

```rust
std::panic::panic_any(MyError::NotAuthorized);
```

## Common Mistakes

- Uni `Result` o'rniga exception transporti sifatida ishlatish.

## Related Concepts

- [[panic]]
- [[catch-unwind]]
- [[any-trait|Any]]
- [[downcasting]]

## Sources

- [[wiki/sources/rust-for-backend-developers-panic]]

