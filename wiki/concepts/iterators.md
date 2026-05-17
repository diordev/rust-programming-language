---
title: "Iterators"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-09
tags: [rust, iterators, lazy, traits, functional]
source_count: 4
---

# Iterators

## Short Definition

Elementlar ketma-ketligi ustida ish bajarishga imkon beruvchi pattern. `Iterator` trait'ini implement qilgan har qanday turdagi qiymat iterator. Rust'da iteratorlar **lazy** — consume qilinmaguncha hech narsa bajarmaydi.

## Why It Matters

Iterator'lar for-loop'ning oddiy alternativi emas — ular abstraktsiya bepul. Rust iterators, C `for (int i=0; ...)` darajasidagi tezlik bilan ishlaydi (zero-cost abstraction). Qo'shimcha: turli `collect()` targetlari, parallel iteratorlar (Rayon), va chaining imkoniyatlari.

## Mental Model

Iterator — "navbat boshqaruvchi": `next()` har chaqiruvda bitta element beradi, tugaganda `None`. Siz faqat "har element uchun nima qilish kerak"ligini aytasiz — iterator qolganini hal qiladi.

```
v1.iter() → [Some(&1), Some(&2), Some(&3), None, None, ...]
                 ↑
             .next() har safar
```

## Syntax and Examples

```rust
// Iterator trait (soddalashtirilgan)
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}
```

```rust
// Uch xil yaratish
let v = vec![1, 2, 3];
let it1 = v.iter();       // &i32 — v owneri saqlanadi
let it2 = v.into_iter();  // i32  — v consume bo'ladi
let it3 = v.iter_mut();   // &mut i32 — mutable borrow

// for loop — implicit iterator
for val in &v { println!("{val}"); }

// next() to'g'ridan-to'g'ri
let mut it = v.iter();
assert_eq!(it.next(), Some(&1));
assert_eq!(it.next(), Some(&2));
assert_eq!(it.next(), Some(&3));
assert_eq!(it.next(), None);
```

```rust
// Consuming adapter: sum
let total: i32 = vec![1, 2, 3].iter().sum();
assert_eq!(total, 6);

// Iterator adapter + consuming: map + collect
let v2: Vec<i32> = vec![1, 2, 3].iter().map(|x| x + 1).collect();
assert_eq!(v2, vec![2, 3, 4]);

// filter: closure environment'dan capture
fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter().filter(|s| s.size == shoe_size).collect()
}
```

```rust
// map closure o'rniga named function ham qabul qiladi
let list_of_numbers = vec![1, 2, 3];
let list_of_strings: Vec<String> =
    list_of_numbers.iter().map(ToString::to_string).collect();
```

## Common Mistakes

**1. Iterator adapter'ni consume qilmasdan qoldirish:**
```rust
v1.iter().map(|x| x + 1); // warning: iterator is lazy and does nothing
```
Doim consuming adapter bilan tugatish: `.collect()`, `.sum()`, va hokazo.

**2. `iter()` bilan owned qiymat kutish:**
`iter()` reference beradi. `into_iter()` owned qiymat beradi (lekin original collection consume bo'ladi).

**3. `next()` uchun `mut` unutish:**
```rust
let v1_iter = v1.iter();
v1_iter.next(); // E0596: cannot borrow as mutable
// → let mut v1_iter = ...
```
`for` loop bu muammoni avtomatik hal qiladi.

**4. `collect()` uchun type annotation:**
Compiler collect targetini bilishi kerak:
```rust
let v2 = v1.iter().map(...).collect(); // E0282: type annotation needed
let v2: Vec<_> = v1.iter().map(...).collect(); // ok
```

## Related Concepts

- [[consuming-adapters]] (`sum`, `collect`, `count`, `fold`)
- [[iterator-adapters]] (`map`, `filter`, `zip`, `enumerate`)
- [[closures]] — iterator metodlari closure qabul qiladi
- [[function-pointers|function pointers]] — named functionlar `map` callbacki bo'lishi mumkin
- [[map-with-named-functions|map with named functions]]
- [[traits]] — `Iterator` standart trait
- [[zero-cost-abstractions]] — iterator vs for-loop tezlik farqi yo'q
- [[for-loop]] — `for` loop ichida implicit iterator ishlatiladi
- [[associated-types|associated types]] — `Iterator::Item` placeholder

## Sources

- [[13-2-processing-a-series-of-items-with-iterators|13.2 Iterators]]
- [[13-functional-language-features-iterators-and-closures|13. Intro]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits — associated types]]
- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
