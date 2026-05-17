---
title: "Dangling Reference"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, borrowing, memory-safety]
source_count: 2
---

# Dangling Reference

## Short Definition

Dangling reference invalid memoryga ishora qiladigan reference. Rust bunday reference yaratadigan code'ni compile qilmaydi.

## Why It Matters

Dangling pointers ko'p systems languagesda use-after-free bugsga olib keladi. Rust references har doim valid bo'lishi kerak degan qoida bilan buni oldini oladi.

## Mental Model

Local value function tugaganda dropped bo'ladi. Shu local value'ga reference qaytarish invalid:

```rust
fn dangle() -> &String {
    let s = String::from("hello");
    &s
}
```

Fix: owned value qaytarish.

```rust
fn no_dangle() -> String {
    let s = String::from("hello");
    s
}
```

Function imzosi ham dangling xavfni ko'rsatishi mumkin. Masalan, ikki `&str` olib `&str` qaytaradigan function relationni aytmasa [[e0106-missing-lifetime-specifier|E0106]] beradi.

## Common Mistakes

- Function ichida yaratilgan local value'ga reference return qilish.
- Lifetime errorni faqat syntax muammosi deb o'ylash; aslida borrowed data valid manbaga ega emas.

## Related Concepts

- [[reference]]
- [[borrowing]]
- [[lifetimes]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]

## Sources

- [[4-2-references-and-borrowing]]
- [[wiki/sources/rust-for-backend-developers-lifetimes]]
