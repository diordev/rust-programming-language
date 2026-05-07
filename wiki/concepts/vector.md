---
title: "Vec<T>"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, collections, vectors]
source_count: 1
---

# Vec<T>

## Short Definition

`Vec<T>` yoki vector bir xil typedagi qiymatlarni memoryda yonma-yon saqlaydigan growable collection.

## Why It Matters

Vector Rustda dynamic list uchun asosiy default collection. Text file lines, shopping cart prices, IDs ro'yxati kabi runtime'da uzunligi o'zgaradigan data uchun ishlatiladi.

## Mental Model

`Vec<T>` stackda metadata, heapda contiguous elements saqlaydi. `T` element type'ini bildiradi; bitta vector faqat bitta concrete `T` saqlaydi.

`push` vector oxiriga element qo'shadi. Agar capacity yetmasa, vector yangi heap memory ajratib, eski elementlarni ko'chirishi mumkin. Shu sabab elementlarga active references bor paytda vectorni mutate qilish borrowing rules bilan cheklanadi.

## Syntax and Examples

```rust
let v: Vec<i32> = Vec::new();
```

```rust
let v = vec![1, 2, 3];
```

```rust
let mut v = Vec::new();
v.push(5);
v.push(6);
```

```rust
for value in &v {
    println!("{value}");
}
```

## Common Mistakes

- Bir vector ichida arbitrary har xil typelarni saqlash mumkin deb o'ylash.
- Empty `Vec::new()`da compiler har doim element type'ni topadi deb kutish.
- Element reference active bo'lgan paytda `push` safe bo'ladi deb o'ylash.
- Out-of-bounds indexing `None` qaytaradi deb o'ylash; indexing panic qiladi, `get` esa `Option` qaytaradi.

## Related Concepts

- [[collections]]
- [[vec-macro|vec! macro]]
- [[vector-indexing|vector indexing]]
- [[generics]]
- [[type-inference|type inference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[drop]]

## Sources

- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
