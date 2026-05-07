---
title: "Struct Update Syntax"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, structs, ownership]
source_count: 1
---

# Struct Update Syntax

## Short Definition

Struct update syntax `..other_instance` orqali yangi struct instance yaratishda qolgan fieldsni boshqa instance'dan olish usuli.

## Why It Matters

Ko'p field bir xil qolib, ozgina field o'zgarganda boilerplateni kamaytiradi. Shu bilan birga [[ownership]] va [[move-semantics|move semantics]]ni amalda ko'rsatadi.

## Mental Model

Explicit berilgan fields yangi value oladi. Qolgan fields `..user1`dan olinadi. Agar field type `Copy` bo'lsa copy qilinadi; `String` kabi move type bo'lsa moved bo'ladi.

## Syntax and Examples

```rust
let user2 = User {
    email: String::from("another@example.com"),
    ..user1
};
```

`..user1` oxirida keladi, chunki undan keyin qaysi fields qolganini aniqlash kerak.

## Common Mistakes

- `..user1` fieldsni clone qiladi deb o'ylash.
- Move qilingan `String` fieldlardan keyin oldingi instance butunicha valid qoladi deb kutish.
- `..user1`ni struct literal o'rtasida yozish.

## Related Concepts

- [[structs]]
- [[struct-instances|struct instances]]
- [[ownership]]
- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[string-type|String]]

## Sources

- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
