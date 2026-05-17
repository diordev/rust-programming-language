---
title: "Ref Pattern"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, patterns, borrowing]
source_count: 1
---

# Ref Pattern

## Short Definition

`ref` yoki `ref mut` pattern ichida value'ni move qilmay, reference sifatida bindingga olish usuli.

## Why It Matters

Pattern match by value qilinayotgan joyda fieldni ko'chirib yubormasdan o'qish yoki o'zgartirish kerak bo'lsa, `ref` foydali. Bu especially struct fieldlari `String` kabi non-`Copy` bo'lsa seziladi.

## Mental Model

`ref` patternning ichida "shu binding value emas, reference bo'lsin" deydi. Bu `&pattern` bilan bir xil emas: `&pattern` kirish qiymati reference ekanini match qiladi, `ref pattern` esa bindingni reference shaklida yaratadi.

## Syntax and Examples

```rust
match person {
    Person { ref name, .. } => println!("{name}"),
}
```

```rust
match person {
    Person { ref mut name, .. } => {
        *name = "John Doe".to_string();
    }
}
```

## Common Mistakes

- `ref`ni oddiy borrow syntax `&value` bilan bir xil deb o'ylash.
- `ref mut` olgandan keyin `*` dereference kerakligini unutish.
- `match &mut value` varianti ba'zan sodda ekanini hisobga olmaslik.

## Related Concepts

- [[pattern-matching|pattern matching]]
- [[match]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[dereference-operator|dereference operator]]

## Sources

- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
