---
title: "E0499 multiple mutable borrows"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, borrowing]
source_count: 1
---

# E0499 multiple mutable borrows

## Symptom

Bir value'ga bir vaqtning o'zida ikki mutable reference yaratishga uringanda `error[E0499]: cannot borrow as mutable more than once at a time` chiqadi.

## Cause

Rust borrowing rule: bir vaqtning o'zida faqat bitta mutable reference bo'lishi mumkin. Bu data racesni compile time'da oldini oladi.

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

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[mutable-reference|mutable reference]]
- [[data-race|data race]]
- [[borrowing]]
