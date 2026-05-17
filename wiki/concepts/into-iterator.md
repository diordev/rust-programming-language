---
title: "IntoIterator"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, iterators, traits, ownership]
source_count: 1
---

# IntoIterator

## Short Definition

`IntoIterator` `for` loop yoki `.into_iter()` orqali qiymatni iteratorga aylantirish contracti. Collectionning o'zi iterator bo'lmasa ham, iterator ishlab bera olishini bildiradi.

## Why It Matters

Rust'da `for` bevosita collection ustida emas, `IntoIterator` ustida ishlaydi. Shu sabab `for x in v`, `for x in &v`, va `for x in &mut v` ownership jihatdan boshqa-boshqa behavior beradi.

## Mental Model

`Iterator` "keyingi itemni ber" desa, `IntoIterator` "mendan iterator yasab ber" deydi. Birinchisi traversal state'ni olib yuradi, ikkinchisi traversalni boshlash eshigi.

## Syntax and Examples

```rust
pub trait IntoIterator {
    type Item;
    type IntoIter: Iterator<Item = Self::Item>;

    fn into_iter(self) -> Self::IntoIter;
}
```

```rust
let v = vec![1, 2, 3];

for x in &v {
    println!("{x}"); // x: &i32
}

for x in v {
    println!("{x}"); // x: i32
}
```

Custom collection:

```rust
impl<'a, T> IntoIterator for &'a MyVec<T> {
    type Item = &'a T;
    type IntoIter = MyVecIter<'a, T>;

    fn into_iter(self) -> Self::IntoIter {
        self.iter()
    }
}
```

## Common Mistakes

- `iter()` va `into_iter()`ni bir xil narsa deb o'ylash.
- `for x in collection` collectionni consume qilishi mumkinligini unutish.
- `IntoIterator`ni iteratorning o'zi bilan chalkashtirish.

## Related Concepts

- [[iterators]]
- [[for-loop|for loop]]
- [[range]]
- [[ownership]]
- [[borrowing]]
- [[vector|Vec<T>]]

## Sources

- [[wiki/sources/rust-for-backend-developers-iterators]]
