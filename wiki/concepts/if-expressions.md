---
title: "if Expressions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, control-flow]
source_count: 1
---

# if Expressions

## Short Definition

`if` expression condition `true` bo'lsa bir blockni, aks holda optional `else` blockni bajaradi. Rustda `if` expression value return qilishi mumkin.

## Why It Matters

Rust conditionals explicit va type-safe. Condition faqat `bool`; truthy/falsy automatic conversion yo'q.

## Mental Model

```rust
let number = if condition { 5 } else { 6 };
```

Bu yerda `if` expressionning har bir arm'i bir xil type qaytarishi kerak.

## Syntax and Examples

```rust
if number != 0 {
    println!("number was something other than zero");
}
```

```rust
if number % 4 == 0 {
    println!("number is divisible by 4");
} else if number % 3 == 0 {
    println!("number is divisible by 3");
} else {
    println!("number is not divisible by 4 or 3");
}
```

## Common Mistakes

- `if number { ... }` yozish; Rust integerni boolga convert qilmaydi.
- `if condition { 5 } else { "six" }` kabi incompatible arm types.
- Juda ko'p `else if` bilan code'ni chigallashtirish; bunday joyda [[match]] ko'pincha yaxshiroq.

## Related Concepts

- [[control-flow|control flow]]
- [[boolean-type|bool]]
- [[statements-and-expressions|statements and expressions]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[3-5-control-flow-the-rust-programming-language]]
