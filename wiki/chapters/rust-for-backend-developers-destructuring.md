---
title: "Rust for Backend Developers: Destructuring"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, patterns, chapter]
source_count: 1
---

# Rust for Backend Developers: Destructuring

## Learning Goal

Tuple, array, struct, va function parameterlarida pattern destructuring qanday ishlashini tushunish va array/slice chegarasini aniq ko'rish.

## Main Ideas

- Destructuring pattern orqali murakkab value ichidan kerakli qismlarni bindinglarga ajratadi.
- Tuple va array positional, struct esa field name asosida destructure qilinadi.
- `_`, `..`, va `rest @ ..` ignored yoki collected qismlarni boshqaradi.
- Array destructuring compile-time lengthga tayanadi; umumiy slice/vector uchun shu model plain bindingda ishlamaydi.
- Function parameter ham pattern position bo'lgani uchun destructuring signature'da ham ishlaydi.

## Concepts

- [[pattern-destructuring|pattern destructuring]]
- [[tuple]]
- [[array]]
- [[array-destructuring|array destructuring]]
- [[structs]]
- [[discarded-binding]]
- [[slice-patterns|slice patterns]]

## Examples

```rust
let employee = ("John Doe", 1980, true);
let (name, birth_year, is_active_employee) = employee;
```

```rust
let [first, _, third, rest @ ..] = [1, 2, 3, 4, 5];
```

## Exercises

- Nested tuple va struct destructuringni bitta misolda qo'llang.
- Fixed-size packet header'ni array destructuring bilan ajrating.

## Review Questions

- Nega array destructuring ishlaydi, lekin slice uchun shu ko'rinish har doim ishlamaydi?
- Struct destructuringda field nomlari readability'ni qanday oshiradi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-destructuring]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[pattern-destructuring|pattern destructuring]]
- [[array-destructuring|array destructuring]]
