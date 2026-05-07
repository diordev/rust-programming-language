---
title: "Ownership"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, ownership, memory-safety]
source_count: 6
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

## Common Mistakes

- Move bo'lgan value'ni yana ishlatishga urinish.
- `clone()`siz heap data deep copied bo'ladi deb o'ylash.
- Functionga `String` berish ownershipni transfer qilishini unutish.
- `HashMap::insert` owned key/value'ni map ichiga move qilishini unutish.

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

- [[4-understanding-ownership-the-rust-programming-language]]
- [[4-1-what-is-ownership-the-rust-programming-language]]
- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[4-3-the-slice-type-the-rust-programming-language]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
