---
title: "drop"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, ownership, memory]
source_count: 2
---

# drop

## Short Definition

`drop` Rust value scope'dan chiqqanda cleanup qilish uchun avtomatik chaqiriladigan function/pattern.

## Why It Matters

Heap allocation ishlatadigan values, masalan `String`, owner scope'dan chiqqanda memoryni allocatorga qaytarishi kerak. Rust buni deterministic tarzda qiladi.

## Mental Model

```rust
{
    let s = String::from("hello");
} // s goes out of scope; drop is called
```

## Syntax and Examples

`drop` odatda qo'lda chaqirilmaydi; ownership/scope tugaganda Rust chaqiradi. `String` author'i cleanup code'ni drop logiciga joylaydi.

Vector scope'dan chiqqanda vectorning o'zi va ichidagi elementlar dropped bo'ladi:

```rust
{
    let v = vec![1, 2, 3, 4];
} // v and its elements are dropped here
```

## Common Mistakes

- Garbage collector bor deb o'ylash.
- Move qilingan oldingi binding drop qiladi deb o'ylash; ownership boshqa bindingga o'tgan bo'ladi.
- Double free Rustda runtimegacha yetadi deb o'ylash; compiler move rules orqali oldini oladi.

## Related Concepts

- [[ownership]]
- [[scope]]
- [[string-type|String]]
- [[vector|Vec<T>]]
- [[move-semantics|move semantics]]

## Sources

- [[4-1-what-is-ownership-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
