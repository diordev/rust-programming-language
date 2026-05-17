---
title: "Attribute"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, syntax, metadata]
source_count: 1
---

# Attribute

## Short Definition

Rustdagi `attribute` bu `#[...]` yoki `#![...]` shaklidagi compile-time metadata/instruction.

## Why It Matters

`derive`, `cfg`, `test`, `allow`, va procedural macro oilasi attribute orqali ulanadi. Ular code behaviorini, compilation scope'ini, yoki code generationni boshqaradi.

## Mental Model

Attribute declarationning o'ziga qo'shiladigan compiler signalidir. Ba'zilari faqat metadata beradi, ba'zilari conditional compilation qiladi, ba'zilari esa code generate qiladi. `#[derive(...)]` shu oilaning bitta maxsus ko'rinishi, xolos.

## Syntax and Examples

```rust
#[derive(Debug, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}
```

```rust
#[cfg(test)]
mod tests {
    // ...
}
```

## Common Mistakes

- Har bir attribute code generate qiladi deb o'ylash.
- `derive`ni attribute oilasidan ajratib, alohida sintaksis deb ko'rish.
- Attribute semantic qarorni compiler o'zi tanlaydi deb o'ylash.

## Related Concepts

- [[derive-attribute|derive attribute]]
- [[cfg-test|cfg(test)]]
- [[custom-derive-macros|custom derive macros]]
- [[attribute-like-macros]]
- [[macro]]

## Sources

- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
