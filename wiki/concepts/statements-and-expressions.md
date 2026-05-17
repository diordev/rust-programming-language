---
title: "Statements and Expressions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, expressions]
source_count: 4
---

# Statements and Expressions

## Short Definition

Statement action bajaradi va value return qilmaydi. Expression valuega evaluate bo'ladi.

## Why It Matters

Rust expression-based language. Function return values, `if` expressions, block expressions va semicolon behaviorini tushunish uchun bu farq zarur.

## Mental Model

- `let y = 6;` statement.
- `5 + 6` expression.
- Function call expression.
- Macro call expression.
- `{ ... }` block expression bo'lishi mumkin.

## Syntax and Examples

```rust
let y = {
    let x = 3;
    x + 1
};
```

`x + 1` oxirida semicolon yo'q, shuning uchun block value return qiladi.

```rust
fn plus_one(x: i32) -> i32 {
    x + 1
}
```

Semicolon qo'shilsa expression statementga aylanadi va ko'pincha `()` chiqadi:

```rust
let unit_value: () = {
    let x = 3;
    x + 1;
};
```

Xuddi shu qoida `if` branches uchun ham ishlaydi:

```rust
let abs_value = if x < 0 { -x } else { x };
```

Declarative macros ham aynan shu syntactic kategoriyalarni farqlaydi: `expr`, `stmt`, `block`, `item` fragmentlari bir xil narsa emas.

## Common Mistakes

- Final expressionga semicolon qo'yish.
- `let` statement value return qiladi deb o'ylash.
- `if` arms turli type qaytarishi mumkin deb o'ylash.

## Related Concepts

- [[functions]]
- [[if-expressions|if expressions]]
- [[unit-type|unit type]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[3-3-functions]]
- [[wiki/sources/rust-for-backend-developers-scopes]]
- [[wiki/sources/rust-for-backend-developers-if]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
