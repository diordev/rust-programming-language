---
title: "Mutable Reference"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, borrowing]
source_count: 2
---

# Mutable Reference

## Short Definition

Mutable reference borrowed value'ni mutate qilishga ruxsat beradigan reference. Syntax: `&mut value`.

## Why It Matters

Mutation kerak bo'lgan joyda ownershipni transfer qilmasdan value'ni o'zgartirish imkonini beradi. Rust mutable referencesni qat'iy cheklab data racesni oldini oladi.

## Mental Model

At any time:

- yoki bitta mutable reference;
- yoki istalgancha immutable references.

Bu ikkalasi overlap qilmaydi.

## Syntax and Examples

```rust
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Mutable vector iteration elementlarga mutable references beradi:

```rust
let mut v = vec![100, 32, 57];

for i in &mut v {
    *i += 50;
}
```

## Common Mistakes

- Original bindingni `mut` qilmasdan `&mut` olishga urinish.
- Bir vaqtning o'zida ikki `&mut` yaratish.
- Immutable borrow active bo'lgan paytda `&mut` olish.
- Mutable reference ortidagi value'ni o'zgartirish uchun kerak joyda `*` ishlatishni unutish.

## Related Concepts

- [[borrowing]]
- [[reference]]
- [[data-race|data race]]
- [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[dereference-operator|dereference operator]]
- [[vector|Vec<T>]]

## Sources

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
