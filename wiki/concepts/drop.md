---
title: "drop"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, ownership, memory, smart-pointers]
source_count: 8
---

# drop

## Short Definition

`drop` scope tugaganda value cleanup bo'lishi modelidir; custom cleanup `Drop` trait orqali yoziladi.

## Why It Matters

Memory, lock, channel, thread join, file handle kabi resource'lar deterministic tarzda tozalanadi.

## Mental Model

Value owner scope'dan chiqqanda cleanup ishlaydi. `Drop::drop`ni qo'lda chaqirib bo'lmaydi; erta cleanup kerak bo'lsa `std::mem::drop` ishlatiladi.

Smart pointerlarda bu heap cleanup uchun, concurrency kodda shutdown uchun, filesystem'da esa file handle close/cleanup uchun ishlaydi.

Advance filesystem source buni file bilan aniq ko'rsatadi: `File` scope'dan chiqqanda handle yopiladi. Lekin bu explicit `flush()`ni keraksiz qilmaydi; faqat close call odatda alohida yozilmasligini bildiradi.

## Syntax and Examples

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("dropping {}", self.data);
    }
}
```

```rust
drop(lock_guard);
```

## Common Mistakes

- Garbage collector bor deb o'ylash.
- `Drop::drop`ni qo'lda chaqirishga urinish.
- Cleanup faqat memory bilan bog'liq deb o'ylash.

## Related Concepts

- [[ownership]]
- [[scope]]
- [[smart-pointers]]
- [[thread-pool]]
- [[graceful-shutdown]]
- [[file-io]]

## Sources

- [[4-1-what-is-ownership]]
- [[8-1-storing-lists-of-values-with-vectors]]
- [[wiki/sources/15-smart-pointers]]
- [[15-3-running-code-on-cleanup-with-the-drop-trait]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
- [[wiki/sources/rust-for-backend-developers-ownership]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
- [[wiki/sources/rust-for-backend-developers-file-system]]
