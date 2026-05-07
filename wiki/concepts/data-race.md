---
title: "Data Race"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, concurrency, memory-safety]
source_count: 1
---

# Data Race

## Short Definition

Data race bir data'ga bir vaqtning o'zida bir nechta pointer access qilganda, kamida bittasi write qilganda, va synchronization yo'q bo'lganda yuz beradi.

## Why It Matters

Data races undefined behaviorga olib keladi. Rust borrowing rules bir vaqtda aliasing va mutationni cheklab, data racesni compile time'da oldini oladi.

## Mental Model

Rust rule:

- bitta mutable reference;
- yoki ko'p immutable references;
- lekin ikkalasi bir vaqtda emas.

## Syntax and Examples

Invalid:

```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;
```

## Common Mistakes

- Multiple mutable aliases xavfsiz deb o'ylash.
- Immutable readers active bo'lgan paytda writer olishga urinish.

## Related Concepts

- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]

## Sources

- [[4-2-references-and-borrowing-the-rust-programming-language]]
