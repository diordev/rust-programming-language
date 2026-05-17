---
title: "Boolean Type"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, types]
source_count: 1
---

# Boolean Type

## Short Definition

`bool` ikki valuega ega Rust scalar type: `true` va `false`.

## Why It Matters

Rust `if`, `while`, va boshqa conditions uchun faqat `bool` qabul qiladi; integer truthiness yo'q.

## Mental Model

Condition expression albatta true/false natija berishi kerak.

## Syntax and Examples

```rust
let active: bool = true;
if active {
    println!("active");
}
```

## Common Mistakes

- `if number { ... }` kabi integer condition yozish.
- `bool`ni string yoki integer bilan aralashtirish.

## Related Concepts

- [[scalar-types|scalar types]]
- [[if-expressions|if expressions]]
- [[control-flow|control flow]]

## Sources

- [[3-2-data-types]]
