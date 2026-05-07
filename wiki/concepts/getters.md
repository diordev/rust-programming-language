---
title: "Getters"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, methods, api-design]
source_count: 1
---

# Getters

## Short Definition

Getter field value'ni o'qish uchun method beradigan API pattern.

## Why It Matters

Rust struct fields uchun gettersni avtomatik yaratmaydi. Getter public API orqali read-only access berish va fieldni private saqlash uchun foydali bo'lishi mumkin.

## Mental Model

`rect.width` field access; `rect.width()` method call. Method field bilan bir xil nomga ega bo'lishi mumkin.

## Syntax and Examples

```rust
impl Rectangle {
    fn width(&self) -> bool {
        self.width > 0
    }
}
```

## Common Mistakes

- Rust fieldlar uchun automatic getter yaratadi deb o'ylash.
- Parentheses bor-yo'qligi field va methodni farqlashini unutish.

## Related Concepts

- [[methods]]
- [[method-syntax|method syntax]]
- [[struct-fields|struct fields]]
- [[api-design|API design]]

## Sources

- [[5-3-methods-the-rust-programming-language]]
