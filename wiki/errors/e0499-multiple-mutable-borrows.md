---
title: "E0499 multiple mutable borrows"
type: error
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, compiler-error, borrowing]
source_count: 2
---

# E0499 multiple mutable borrows

## Symptom

Bir value'ga bir vaqtning o'zida ikki mutable reference yaratishga uringanda `error[E0499]: cannot borrow as mutable more than once at a time` chiqadi.

## Cause

Rust borrowing rule: bir vaqtning o'zida faqat bitta mutable reference bo'lishi mumkin. Bu data racesni compile time'da oldini oladi.

Backend beginner ownership source bu qoidani referential safety formulasi sifatida aniq yozadi: yo bitta `&mut T`, yo ko'p `&T`.

## Fix Pattern

Mutable borrow scopes overlap qilmasin:

```rust
let mut s = String::from("hello");

{
    let r1 = &mut s;
} // r1 ends here

let r2 = &mut s;
```

## Minimal Example

Failing:

```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;

println!("{r1}, {r2}");
```

## Related Concepts

- [[4-2-references-and-borrowing]]
- [[mutable-reference|mutable reference]]
- [[data-race|data race]]
- [[borrowing]]
- [[wiki/sources/rust-for-backend-developers-ownership]]
