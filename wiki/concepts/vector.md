---
title: "Vec<T>"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, collections, vectors]
source_count: 3
---

# Vec<T>

## Short Definition

`Vec<T>` yoki vector bir xil typedagi qiymatlarni memoryda yonma-yon saqlaydigan growable collection.

## Why It Matters

Vector Rustda dynamic list uchun asosiy default collection. Text file lines, shopping cart prices, IDs ro'yxati kabi runtime'da uzunligi o'zgaradigan data uchun ishlatiladi.

## Mental Model

`Vec<T>` stackda metadata, heapda contiguous elements saqlaydi. `T` element type'ini bildiradi; bitta vector faqat bitta concrete `T` saqlaydi.

`push` vector oxiriga element qo'shadi. Agar capacity yetmasa, vector yangi heap memory ajratib, eski elementlarni ko'chirishi mumkin. Shu sabab elementlarga active references bor paytda vectorni mutate qilish borrowing rules bilan cheklanadi.

Backend beginner source bu modelni `pointer + len + capacity` uchligi bilan juda toza beradi va `Vec::with_capacity(n)`ni shu mental modelga bog'laydi.

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
let mut v = Vec::with_capacity(3);
v.push(1);
v.push(2);
v.push(3);
v.push(4); // reallocation bo'lishi mumkin
```

```rust
for value in &v {
    println!("{value}");
}
```

`drain(..)` bilan ownership chiqarish:

```rust
for worker in self.workers.drain(..) {
    worker.thread.join().unwrap();
}
```

`drain(..)` elementlarni vector ichidan olib tashlaydi va ularni owned iterator sifatida beradi. Bu cleanup kodda, ayniqsa `Drop` ichida, borrowed `self`dan ownership chiqarish kerak bo'lganda foydali.

## Common Mistakes

- Bir vector ichida arbitrary har xil typelarni saqlash mumkin deb o'ylash.
- Empty `Vec::new()`da compiler har doim element type'ni topadi deb kutish.
- Element reference active bo'lgan paytda `push` safe bo'ladi deb o'ylash.
- Out-of-bounds indexing `None` qaytaradi deb o'ylash; indexing panic qiladi, `get` esa `Option` qaytaradi.
- `drain(..)` faqat delete operation deb o'ylash — u ownership transfer iteratori hamdir.

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
- [[graceful-shutdown]]

## Sources

- [[8-1-storing-lists-of-values-with-vectors]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
- [[wiki/sources/rust-for-backend-developers-vector]]
