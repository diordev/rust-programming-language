---
title: "Collections"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, collections]
source_count: 4
---

# Collections

## Short Definition

Collections bir nechta value'ni bitta data structure ichida saqlaydigan standard library typelaridir.

## Why It Matters

Real programlarda data ko'pincha bitta value emas, ro'yxat, text, yoki key-value mapping sifatida keladi. Collections runtime'da grow/shrink qiladigan heap-backed structures orqali bunday data bilan ishlashni beradi.

## Mental Model

`array` va `tuple` compile-time shape'ga ega built-in compound types. Common collections esa standard library typelari bo'lib, ko'pincha heap allocation ishlatadi va program ishlashi davomida hajmini o'zgartira oladi.

Chapter 8 uchta common collectionni ochadi:

- [[vector|Vec<T>]]: same-type values ro'yxati.
- [[string-type|String]]: growable UTF-8 text.
- [[hash-map|HashMap]]: key-value association.

`String` collection sifatida bytes (`Vec<u8>`) ustidagi text-specific wrapper bo'lib, UTF-8 guarantees beradi.

`HashMap<K, V>` index emas, key orqali lookup qiladi. User ID, team name, word count, setting key kabi meaningful identifier bilan data topish kerak bo'lsa, vector indexidan ko'ra hash map modeli mosroq.

## Syntax and Examples

```rust
let numbers = vec![1, 2, 3];
let text = String::from("hello");
```

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
```

## Common Mistakes

- Runtime'da o'sadigan list uchun fixed-size [[array]] ishlatish.
- Har bir collectionning capability va cost'i bir xil deb o'ylash.
- Heap-backed collection mutationi existing referencesga ta'sir qilishi mumkinligini unutish.
- `String`ni collection emas, faqat primitive text type deb o'ylash.
- Key-based lookup kerak bo'lgan joyda vector indexlarini artificial ID sifatida ishlatish.

## Related Concepts

- [[standard-library|standard library]]
- [[vector|Vec<T>]]
- [[string-type|String]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[utf-8|UTF-8]]
- [[array]]
- [[tuple]]
- [[stack-and-heap|stack and heap]]
- [[ownership]]
- [[borrowing]]

## Sources

- [[8-common-collections-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
