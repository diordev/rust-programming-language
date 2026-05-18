---
title: "Thread Local Storage"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, concurrency, tls, thread-local]
source_count: 1
---

# Thread Local Storage

## Short Definition

Thread-local storage har thread uchun alohida nusxasi bo'ladigan "globalga o'xshash" state modeli. Rust'da `thread_local!` macro bilan beriladi.

## Why It Matters

Per-thread context'ni function signature'lariga uzatmasdan saqlash mumkin.

## Mental Model

Variable nomi bitta, lekin har thread o'z private katagiga yozadi.

## Syntax and Examples

```rust
thread_local! {
    static NUM: std::cell::Cell<u32> = const { std::cell::Cell::new(0) };
}
```

## Common Mistakes

- TLS'ni ordinary global mutable state bilan bir xil deb o'ylash.
- Hidden dependency'larni ko'paytirib yuborish.

## Related Concepts

- [[cell-t|Cell<T>]]
- [[threads]]
- [[sync-trait|Sync trait]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
