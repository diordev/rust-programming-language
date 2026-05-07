---
title: "for Loop"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, control-flow]
source_count: 2
---

# for Loop

## Short Definition

`for` loop iterator yoki collection elementlari bo'ylab yurish uchun ishlatiladi.

## Why It Matters

Rustda collection iteration uchun `for` odatda eng safe va idiomatic loop.

## Mental Model

`for` har iterationda navbatdagi itemni bindingga beradi va collection chegaralarini qo'lda boshqarishni kamaytiradi.

## Syntax and Examples

```rust
for number in [1, 2, 3] {
    println!("{number}");
}
```

Vector bo'ylab references bilan yurish:

```rust
let v = vec![100, 32, 57];
for value in &v {
    println!("{value}");
}
```

Mutable vector iteration:

```rust
let mut v = vec![100, 32, 57];
for value in &mut v {
    *value += 50;
}
```

## Common Mistakes

- Index bilan manual loop yozib bounds xatolarini ko'paytirish.
- Ownership va borrowing iterationga ta'sir qilishini unutish.
- Iteration paytida vectorni insert/remove qilishga urinish.

## Related Concepts

- [[loop]]
- [[while-loop|while loop]]
- [[control-flow|control flow]]
- [[vector|Vec<T>]]
- [[mutable-reference|mutable reference]]
- [[dereference-operator|dereference operator]]

## Sources

- [[3-5-control-flow-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
