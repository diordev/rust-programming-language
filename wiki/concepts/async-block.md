---
title: "Async Block"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async]
source_count: 1
---

# Async Block

## Short Definition

`async { ... }` - block ichidagi kodni [[future|Future]]ga aylantiradigan Rust ifodasi.

## Why It Matters

Async block nomli `async fn` yozmasdan future yaratishga imkon beradi. Bu `block_on`, `spawn`, `join`, testlar, va kichik async compositionlarda qulay.

## Mental Model

Oddiy block `{ 1 }` darhol `1` qaytaradi. Async block `async { 1 }` esa darhol future qaytaradi; `1` natijasi future bajarilganda olinadi.

## Syntax and Examples

```rust
let future = async { 1 };
let result = futures::executor::block_on(future);
assert_eq!(result, 1);
```

`async move` capture qilingan qiymatlarni future ichiga ko'chiradi:

```rust
let text = String::from("salom");
let future = async move {
    text.len()
};
```

## Common Mistakes

- Async block ichidagi kod darhol bajariladi deb o'ylash. Future `await` yoki executor orqali poll qilinmaguncha ishlamaydi.
- Borrow qilingan qiymat async blockdan uzoqroq yashaydi deb taxmin qilish. `async move` kerak bo'lishi mumkin.
- Bitta async block ichidagi sequential `.await`larni avtomatik parallel deb qabul qilish.

## Related Concepts

- [[async-await|async/await]]
- [[async-move-block|async move block]]
- [[async-closure|async closure]]
- [[future|Future]]
- [[async-state-machine|async state machine]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

