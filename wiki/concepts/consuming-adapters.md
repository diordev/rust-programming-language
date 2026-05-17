---
title: "Consuming Adapters"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, iterators, consuming-adapters, functional]
source_count: 1
---

# Consuming Adapters

## Short Definition

`Iterator` trait'ida `next()` ni ichida chaqirib, iteratorni **tugata ishlatadigan** metodlar. Chaqirilgandan so'ng iterator qayta foydalanish uchun mavjud bo'lmaydi.

## Why It Matters

Lazy iterator'larni haqiqiy natijaga aylantirish uchun consuming adapter zarur. `map`, `filter` singari iterator adapters faqat yangi lazy iterator beradi — ish bajarilmaydi. Consuming adapter chain'ni "ishga tushiradi".

## Mental Model

```
v.iter().map(...).filter(...)   ← barchasi lazy, hali ish yo'q
                 .collect()     ← shu yerda hamma narsa bajariladi
```

## Syntax and Examples

```rust
let v = vec![1, 2, 3];

// sum — barcha elementlarni yig'adi
let total: i32 = v.iter().sum();
assert_eq!(total, 6);

// collect — collectionga yig'adi
let v2: Vec<i32> = v.iter().map(|x| x * 2).collect();
assert_eq!(v2, vec![2, 4, 6]);

// count — nechta element
let n = v.iter().count();
assert_eq!(n, 3);

// last — oxirgi element
let last = v.iter().last();
assert_eq!(last, Some(&3));

// any — biror element shartga mosmi
let has_two = v.iter().any(|x| *x == 2);
assert!(has_two);

// all — barcha elementlar shartga mosmi
let all_pos = v.iter().all(|x| *x > 0);
assert!(all_pos);

// fold — o'zingiz belgilagan yig'ish logikasi
let product = v.iter().fold(1, |acc, x| acc * x);
assert_eq!(product, 6);
```

## Common Mistakes

**Iterator consume bo'lgandan keyin ishlatish:**
```rust
let it = v.iter();
let s: i32 = it.sum();
for x in it { ... } // E0382: use of moved value
```

**`collect()` uchun type context yo'q:**
```rust
let c = v.iter().collect(); // E0282 — target type noma'lum
let c: Vec<_> = v.iter().collect(); // ok
```

## Related Concepts

- [[iterators]] — consuming adapters iterator ustida ishlaydi
- [[iterator-adapters]] — lazy transformations, consuming adapter bilan tugatiladi
- [[closures]] — `fold`, `any`, `all` closure qabul qiladi

## Sources

- [[13-2-processing-a-series-of-items-with-iterators|13.2 Iterators]]
