---
title: "collect()"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, iterators, collections]
source_count: 2
---

# collect()

## Short Definition

`collect()` — iteratorni concrete collection (masalan `Vec<T>`, `HashMap<K,V>`, `String`) ga aylantiruvchi consuming adapter metod.

## Why It Matters

Rust'da ko'plab operatsiyalar iterator qaytaradi — `map`, `filter`, `zip` va boshqalar. Ularni haqiqiy collection sifatida ishlatish uchun `collect()` kerak. Type annotation majburiy, chunki kompilyator qaysi collectionni qurish kerakligini bilmaydi.

## Mental Model

Iterator — lazy konveyer. `collect()` — konveyer oxirida joylashgan qop: u konveyer buyumlarini to'plab container ga joylashtiradi. Qaysi container — type annotationdan biladi.

## Syntax and Examples

```rust
use std::env;

let args: Vec<String> = env::args().collect();
```

Type annotation `Vec<String>` — `collect()` ga qaysi container yasashni ko'rsatadi.

```rust
let doubled: Vec<i32> = vec![1, 2, 3]
    .iter()
    .map(|x| x * 2)
    .collect();
// [2, 4, 6]
```

`HashMap` ham yasaladi:

```rust
use std::collections::HashMap;

let map: HashMap<_, _> = vec![("a", 1), ("b", 2)]
    .into_iter()
    .collect();
```

Iterator lazy bo'lsa, `collect()` ko'pincha computation boshlanadigan nuqta bo'ladi:

```rust
use std::collections::HashSet;

let squares: HashSet<i32> = vec![1, 2, 3, 4]
    .into_iter()
    .filter(|x| x % 2 == 0)
    .map(|x| x * x)
    .collect();
```

## Common Mistakes

- Type annotation unutilsa: `error[E0282]: type annotations needed` — kompilyator qaysi collection ekanini aniqlay olmaydi.
- `iter()` bilan `collect::<Vec<&T>>()` — reference lar yig'iladi, owned emas. Owned kerak bo'lsa `.cloned()` yoki `.copied()` kerak.

## Related Concepts

- [[iterators]]
- [[consuming-adapters]]
- [[type-annotations]]
- [[vec-macro|Vec]]
- [[hash-map|HashMap]]

## Sources

- [[12-1-accepting-command-line-arguments]]
- [[wiki/sources/rust-for-backend-developers-iterators]]
