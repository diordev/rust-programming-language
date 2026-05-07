---
title: "13.2. Processing a Series of Items with Iterators"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, iterators, consuming-adapters, iterator-adapters, lazy]
source_count: 1
---

# 13.2. Processing a Series of Items with Iterators

## Source

- URL: https://doc.rust-lang.org/stable/book/ch13-02-iterators.html
- Kitob: The Rust Programming Language
- Bob: 13.2

## Detailed Summary

### Iterator nima?

[[iterators|Iterator]] — elementlar ketma-ketligi ustida ish bajarishga imkon beruvchi pattern. Rust'da iteratorlar **lazy**: faqat consume qilinganida ishga tushadi.

```rust
let v1 = vec![1, 2, 3];
let v1_iter = v1.iter(); // hali hech narsa bajarmadi
for val in v1_iter {    // shu yerda consume bo'ladi
    println!("Got: {val}");
}
```

### Iterator trait

```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
    // default implementatsiyali ko'plab metodlar...
}
```

Faqat `next()` metodi implement qilinishi shart. `Item` — associated type, iterator chiqaradigan element tipi. `next()` har chaqiruvda `Some(element)` qaytaradi, tugaganda `None`.

`next()` ni to'g'ridan-to'g'ri chaqirish uchun iterator `mut` bo'lishi kerak — ichki holat o'zgaradi. `for` loop bu ishni parda ortida amalga oshiradi.

### Uch xil iterator yaratish

| Metod | Qaytaradi | Ownership |
|-------|-----------|-----------|
| `.iter()` | `&T` — immutable reference | Qoladi |
| `.into_iter()` | `T` — owned | Collection o'tib ketadi |
| `.iter_mut()` | `&mut T` — mutable reference | Mutable borrow |

### Consuming adapters (iste'mol qiluvchi)

`next()` ni ichidan chaqiradigan metodlar iterator'ni **consume** qiladi. Keyin `v1_iter`'ni ishlatib bo'lmaydi.

```rust
let v1 = vec![1, 2, 3];
let v1_iter = v1.iter();
let total: i32 = v1_iter.sum(); // v1_iter bu yerda consume bo'ldi
// v1_iter endi foydalanib bo'lmaydi
assert_eq!(total, 6);
```

Asosiy consuming adapters: `sum()`, `collect()`, `count()`, `last()`, `fold()`.

### Iterator adapters (o'zgartiruvchi)

Iterator'ni **consume qilmaydi** — yangi (lazy) iterator qaytaradi. Natija olish uchun oxirida consuming adapter chaqirilishi kerak.

```rust
let v1: Vec<i32> = vec![1, 2, 3];
let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();
assert_eq!(v2, vec![2, 3, 4]);
```

Agar `collect()` bo'lmasa — **warning**: "iterators are lazy and do nothing unless consumed".

### filter() bilan closure'ning environment capture qilishi

```rust
fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter().filter(|s| s.size == shoe_size).collect()
}
```

`filter` closure `shoe_size`'ni environment'dan capture qiladi — bu closures va iterators'ning kuchli birgalikdagi pattern'i.

### Chaining

```rust
v1.iter()
  .filter(|x| **x > 1)
  .map(|x| x * 2)
  .collect::<Vec<_>>()
```

Barchasi lazy — `collect()` chaqirilgunicha hech narsa bajarmaydi.

## Key Concepts

- [[iterators]]
- [[consuming-adapters]] (`sum`, `collect`)
- [[iterator-adapters]] (`map`, `filter`)
- [[closures]] (iterator metodlari closure qabul qiladi)
- [[zero-cost-abstractions]] (iterators for-loopga teng ishlaydi)

## Code Examples

- Listing 13-10..11: Iterator yaratish va `for` loop bilan ishlatish
- Listing 13-12: `next()` to'g'ridan-to'g'ri chaqirish
- Listing 13-13: `sum()` consuming adapter
- Listing 13-14..15: `map()` + `collect()` — lazy warning va to'g'ri ishlatish
- Listing 13-16: `filter()` closure bilan environment capture

## Exercises or Practice Ideas

- `into_iter()`, `iter()`, `iter_mut()` farqlarini amalda sinab ko'ring
- `filter` + `map` + `collect` chain yozing
- `Iterator` trait'ini custom struct uchun implement qiling

## Questions Raised

- `Iterator` trait'i custom type uchun qanday implement qilinadi?
- `fold()` metodi nima qiladi va qachon foydali?
- Parallel iterators (Rayon crate) oddiy iteratorlardan qanday farq qiladi?

## Links To Update

- [[iterators]]
- [[consuming-adapters]]
- [[iterator-adapters]]
- [[13-functional-language-features]] chapter
