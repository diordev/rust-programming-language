---
title: "Iterator Adapters"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, iterators, iterator-adapters, functional, lazy]
source_count: 2
---

# Iterator Adapters

## Short Definition

`Iterator` trait'ida asosiy iteratorni **consume qilmay**, uning ustidan yangi **lazy** iterator qaytaradigan metodlar. Haqiqiy ishni bajarish uchun chain oxirida consuming adapter (`collect`, `sum`, va h.k.) chaqirilishi kerak.

## Why It Matters

Iterator adapters closure bilan birgalikda declarative, chainable transformationlar yozishga imkon beradi. `for` loop o'rniga `filter` + `map` + `collect` — ixcham va xavfsiz.

## Mental Model

```
asl iterator → .map() → yangi lazy iterator
                            ↓
                        .filter() → yana yangi lazy iterator
                                        ↓
                                    .collect() → natija
```

Har bir adapter — funnel (huni). Barchasi `collect()` chaqirilgunicha kutadi.

## Syntax and Examples

```rust
// map: har elementni o'zgartiradi
let v2: Vec<i32> = vec![1, 2, 3].iter().map(|x| x + 1).collect();
assert_eq!(v2, vec![2, 3, 4]);

// filter: shartga mos elementlarni qoldiradi
let evens: Vec<&i32> = vec![1, 2, 3, 4].iter().filter(|x| *x % 2 == 0).collect();
assert_eq!(evens, vec![&2, &4]);

// zip: ikkita iteratorni juftlashtiradi
let names = vec!["Alice", "Bob"];
let scores = vec![90, 85];
let pairs: Vec<_> = names.iter().zip(scores.iter()).collect();
// pairs = [("Alice", 90), ("Bob", 85)]

// enumerate: index + element
for (i, val) in vec!["a", "b"].iter().enumerate() {
    println!("{i}: {val}");
}

// take: birinchi N element
let first_two: Vec<_> = vec![1, 2, 3, 4].iter().take(2).collect();
assert_eq!(first_two, vec![&1, &2]);

// skip: birinchi N elementni o'tkazib yuborish
let skip_two: Vec<_> = vec![1, 2, 3, 4].iter().skip(2).collect();
assert_eq!(skip_two, vec![&3, &4]);

// flat_map: har elementni iteratorga aylantiradi va tekislaydi
let words = vec!["hello world", "foo bar"];
let chars: Vec<&str> = words.iter().flat_map(|s| s.split(' ')).collect();

// filter_map: Option qaytargan transform + filter
fn safe_sqrt(n: f32) -> Option<f32> {
    if n < 0.0 { None } else { Some(n.sqrt()) }
}

let roots: Vec<f32> = [4.0, -25.0, 9.0]
    .into_iter()
    .filter_map(safe_sqrt)
    .collect();
```

```rust
// Chaining: filter + closure environment capture
fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter()
         .filter(|s| s.size == shoe_size)
         .collect()
}
```

## Common Mistakes

**Consume qilmasdan qoldirish:**
```rust
v.iter().map(|x| x + 1); // warning: unused Map that must be used
// Iterators are lazy — collect() qo'shing
```

**`map` ichida return qilishni unutish:**
```rust
let v2: Vec<_> = v.iter().map(|x| { x + 1; }).collect();
// closure unit () qaytaradi, lekin Vec<i32> kutilmoqda
// → map(|x| x + 1) — semicolonsiz
```

## Related Concepts

- [[iterators]] — adapter'lar ustida ishlaydi
- [[consuming-adapters]] — adapter chain'ni tugatadi
- [[closures]] — adapters closure qabul qiladi
- [[option|Option]] — `filter_map` `Some` itemlarni o'tkazadi, `None`ni tashlaydi
- [[zero-cost-abstractions]] — chainlangan adapterlar loop'ga teng samarali

## Sources

- [[13-2-processing-a-series-of-items-with-iterators|13.2 Iterators]]
- [[wiki/sources/rust-for-backend-developers-iterators]]
