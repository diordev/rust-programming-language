---
title: "Borrow Checker"
type: concept
status: stable
created: 2026-05-07
updated: 2026-05-07
tags: [rust, borrowing, lifetimes, memory-safety]
source_count: 3
---

# Borrow Checker

## Short Definition

Borrow checker — Rust compiler ichidagi mexanizm bo'lib, compile time'da barcha referencelar va borrowlarning valid ekanini tekshiradi. U [[ownership]], [[borrowing]], va [[lifetimes]] qoidalarini amalga oshiradi.

## Why It Matters

Borrow checker runtime xatolarisiz [[memory-safety|memory safety]]ni kafolatlaydi. [[dangling-reference|Dangling reference]], [[data-race|data race]], va use-after-free kabi xato sinflari compile time'da rad etiladi — dastur ishga tushmasdan oldin.

## Mental Model

Borrow checker uchta asosiy qoidani tekshiradi:

1. Istalgan vaqtda **yoki** bir nechta immutable reference (`&T`), **yoki** bitta mutable reference (`&mut T`) bo'lishi mumkin — ikkalasi bir vaqtda emas.
2. Barcha referencelar **valid bo'lishi kerak** — ya'ni owner hali mavjud bo'lishi lozim.
3. Reference owner ma'lumotidan **uzoqroq yashay olmaydi** (lifetime qoidasi).

```rust
let mut s = String::from("hello");
let r1 = &s;
let r2 = &s;      // OK: bir nechta immutable reference
// let r3 = &mut s;  // Xato: immutable borrow mavjud bo'lganda mutable borrow bo'lmaydi
println!("{r1}, {r2}");
```

## Common Mistakes

- Immutable reference doirasida mutable borrow olishga urinish → [[e0502-mutable-and-immutable-borrow-conflict]]
- Bir vaqtda ikki mutable borrow → [[e0499-multiple-mutable-borrows]]
- Ownersiz reference qaytarish → [[e0515-cannot-return-local-reference]]
- Lifetime annotatsiyasini tushirib qoldirish → [[e0106-missing-lifetime-specifier]]

## Related Concepts

- [[ownership]]
- [[borrowing]]
- [[lifetimes]]
- [[lifetime-elision]]
- [[dangling-reference]]
- [[data-race]]
- [[memory-safety]]
