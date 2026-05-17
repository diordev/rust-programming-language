---
title: "Ownership"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, ownership, memory-safety]
source_count: 7
---

# Ownership

## Short Definition

Ownership Rust programi memoryni qanday boshqarishini belgilaydigan compile-time rules sistemi.

## Why It Matters

Ownership Rustga garbage collector ishlatmasdan memory safety guarantees berishga yordam beradi. Compiler ownership rulesni buzgan code'ni compile qilmaydi.

## Mental Model

Ownership rules:

- Har bir value'ning owner'i bor.
- Bir vaqtning o'zida faqat bitta owner bo'ladi.
- Owner scope'dan chiqqanda value dropped bo'ladi.

Heap data uchun ownership ayniqsa muhim: kim cleanup qilishini ownership aniqlaydi.

Amaliy move nuqtalari faqat assignment emas: function argumenti, function return value, va keyinroq closure capture ham ownershipni ko'chirishi mumkin.

Ownership bilan bog'liq keyingi model: [[borrowing]] ownershipni transfer qilmasdan value'ga reference orqali kirish imkonini beradi. [[slices]] esa collectionning bir qismiga reference beradi.

[[structs]] ownershipni fieldlar darajasida ham ko'rsatadi. Struct update syntax `..user1` orqali `String` kabi move typelarni yangi instance'ga ko'chirishi mumkin, `bool` yoki `u64` kabi [[copy-trait|Copy trait]] typelar esa copy qilinadi.

[[hash-map|HashMap]] ham ownershipni amalda ko'rsatadi: owned `String` key yoki value `insert` orqali mapga berilsa, ownership mapga move bo'ladi. `i32` kabi `Copy` values esa copied bo'ladi.

## Syntax and Examples

```rust
let s1 = String::from("hello");
let s2 = s1; // ownership moves from s1 to s2
```

```rust
fn takes_ownership(some_string: String) {
    println!("{some_string}");
}
```

```rust
use std::collections::HashMap;

let field_name = String::from("Favorite color");
let field_value = String::from("Blue");

let mut map = HashMap::new();
map.insert(field_name, field_value);

// field_name va field_value endi valid emas.
```

Borrowing bilan ownershipni saqlab qolish:

```rust
fn len_of_string(s: &String) -> usize {
    s.len()
}
```

## Common Mistakes

- Move bo'lgan value'ni yana ishlatishga urinish.
- `clone()`siz heap data deep copied bo'ladi deb o'ylash.
- Functionga `String` berish ownershipni transfer qilishini unutish.
- `HashMap::insert` owned key/value'ni map ichiga move qilishini unutish.
- Deterministic cleanup bor ekan, safe Rust'da leak mutlaqo imkonsiz deb o'ylash.

## Related Concepts

- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[drop]]
- [[scope]]
- [[borrowing]]
- [[reference]]
- [[slices]]
- [[structs]]
- [[struct-update-syntax]]
- [[hash-map|HashMap]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]

## Sources

- [[wiki/sources/4-understanding-ownership]]
- [[4-1-what-is-ownership]]
- [[4-2-references-and-borrowing]]
- [[4-3-the-slice-type]]
- [[5-1-defining-and-instantiating-structs]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[wiki/sources/rust-for-backend-developers-ownership]]
