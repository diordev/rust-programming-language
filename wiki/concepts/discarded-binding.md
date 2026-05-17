---
title: "Discarded Binding"
type: concept
status: active
created: 2026-05-16
updated: 2026-05-17
tags: [rust, variables, bindings]
source_count: 3
---

# Discarded Binding

## Short Definition

Rustdagi `_` bindingi qiymatni ataylab e'tiborsiz qoldirish uchun ishlatiladi. Bu `_name` bilan bir xil emas.

## Why It Matters

Unused value yoki resultni ataylab tashlab yuborayotganingizni kodda ko'rsatadi. Shu bilan birga `_name` va `_`ni aralashtirib yuborish ownership, warnings, va readability bo'yicha noto'g'ri signal beradi.

## Mental Model

- `_name` real binding: compiler unused warningni bosadi, lekin name mavjud.
- `_` discarded binding: qiymat olinadi, lekin name saqlanmaydi va unga keyin murojaat qilib bo'lmaydi.

Backend beginner result source bu patternning juda amaliy ishlatilishini ko'rsatadi: intentionally ignored `Result` uchun `let _ = function_that_may_fail();`. Bu compilerga "ha, men warningni ko'rdim va bu resultni ataylab tashlayapman" degan signal beradi.

## Syntax and Examples

```rust
let _reserved_for_later = 5;
let _ = compute_value();
```

Pattern sifatida ham ishlaydi:

```rust
let (_, status_code) = (request_id, 200);
```

Ignored `Result`:

```rust
let _ = function_that_may_fail();
```

## Common Mistakes

- `_name` va `_` bir xil deb o'ylash.
- Muhim `Result` yoki error signalini `let _ = ...;` bilan bexosdan yutib yuborish.
- `_` bor ekan, ownership umuman ta'sir qilmaydi deb o'ylash.
- Compiler warningini bosish bilan errorni "hal qildim" deb o'ylash.

## Related Concepts

- [[variables-and-mutability|variables and mutability]]
- [[immutability]]
- [[result|Result]]
- [[pattern-matching]]
- [[raw-identifiers]]
- [[error-handling]]

## Sources

- [[wiki/sources/rust-for-backend-developers-variables]]
- [[wiki/sources/rust-for-backend-developers-destructuring]]
- [[wiki/sources/rust-for-backend-developers-result]]
