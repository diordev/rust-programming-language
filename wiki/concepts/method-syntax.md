---
title: "Method Syntax"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, methods, syntax]
source_count: 1
---

# Method Syntax

## Short Definition

Method syntax instance nomidan keyin dot, method name, parentheses, va arguments yozish shakli: `rect1.area()`.

## Why It Matters

Method syntax behaviorni receiver instance bilan bog'lab o'qishni osonlashtiradi va [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]]dan foydalanadi.

## Mental Model

`rect1.area()` "rect1 ustida area methodini chaqir" degani. Rust receiverni method signaturega mos borrow/deref qiladi.

## Syntax and Examples

```rust
let area = rect1.area();
let fits = rect1.can_hold(&rect2);
```

## Common Mistakes

- Method call uchun receiver kerakligini unutish.
- Field access va method callni chalkashtirish: `rect1.width` field, `rect1.width()` method.

## Related Concepts

- [[methods]]
- [[self-parameter|self parameter]]
- [[struct-fields|struct fields]]

## Sources

- [[5-3-methods-the-rust-programming-language]]
