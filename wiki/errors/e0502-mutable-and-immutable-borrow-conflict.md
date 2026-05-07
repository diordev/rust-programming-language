---
title: "E0502 mutable and immutable borrow conflict"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, borrowing]
source_count: 3
---

# E0502 mutable and immutable borrow conflict

## Symptom

Immutable borrow hali active bo'lgan paytda mutable borrow olishga uringanda `error[E0502]: cannot borrow as mutable because it is also borrowed as immutable` chiqadi.

## Cause

Rust immutable readers active bo'lgan paytda writer/mutable borrow'ga ruxsat bermaydi. Immutable reference users value o'zgarmaydi deb kutadi.

Vector contextida bu qoida reallocation bilan bog'lanadi: elementga reference ushlab turgan paytda `push` qilish vectorni yangi heap allocationga ko'chirishi mumkin, shunda eski element reference'i dangling bo'lardi.

## Fix Pattern

Immutable reference'ning last use joyi mutable borrowdan oldin tugasin:

```rust
let mut s = String::from("hello");

let r1 = &s;
let r2 = &s;
println!("{r1} and {r2}");

let r3 = &mut s;
println!("{r3}");
```

## Minimal Example

Failing:

```rust
let mut s = String::from("hello");

let r1 = &s;
let r2 = &s;
let r3 = &mut s;

println!("{r1}, {r2}, and {r3}");
```

Slice-related failing pattern:

```rust
let mut s = String::from("hello world");
let word = first_word(&s);
s.clear();
println!("the first word is: {word}");
```

Vector-related failing pattern:

```rust
let mut v = vec![1, 2, 3, 4, 5];

let first = &v[0];

v.push(6);

println!("The first element is: {first}");
```

## Related Concepts

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[4-3-the-slice-type-the-rust-programming-language]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[slices]]
- [[vector|Vec<T>]]
- [[vector-indexing|vector indexing]]
- [[data-race|data race]]

## Sources

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[4-3-the-slice-type-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
