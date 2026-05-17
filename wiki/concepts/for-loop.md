---
title: "for Loop"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, control-flow]
source_count: 4
---

# for Loop

## Short Definition

`for` loop iterator yoki collection elementlari bo'ylab yurish uchun ishlatiladi.

## Why It Matters

Rustda collection iteration uchun `for` odatda eng safe va idiomatic loop.

## Mental Model

`for` har iterationda navbatdagi itemni bindingga beradi va collection chegaralarini qo'lda boshqarishni kamaytiradi.

Rustdagi `for` C-style `for(init; cond; step)` emas. Numeric iteration kerak bo'lsa range ishlatiladi: `0..10`, `0..=10`.

Muhim detail: `for` collection ustida "sehr" qilmaydi. U `IntoIterator::into_iter(...)` orqali iterator olib, traversalni shu iteratorga topshiradi.

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

Range bilan:

```rust
for i in 0..=10 {
    print!("{i} ");
}
```

Ownership farqi:

```rust
let v = vec![1, 2, 3];

for value in &v {
    println!("{value}"); // &i32
}

for value in v {
    println!("{value}"); // i32, v consume bo'ladi
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
- [[iterators]]
- [[into-iterator|IntoIterator]]
- [[range]]

## Sources

- [[3-5-control-flow]]
- [[8-1-storing-lists-of-values-with-vectors]]
- [[wiki/sources/rust-for-backend-developers-loops]]
- [[wiki/sources/rust-for-backend-developers-iterators]]
