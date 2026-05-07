---
title: "Struct Fields"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, structs]
source_count: 2
---

# Struct Fields

## Short Definition

Struct field struct ichidagi named data piece bo'lib, field name va type'dan iborat.

## Why It Matters

Field names valuesning ma'nosini explicit qiladi. Shu sababli struct [[tuple]]ga qaraganda order dependency va readability muammolarini kamaytiradi.

## Mental Model

Field type struct definitionda belgilanadi; field value esa har bir [[struct-instances|instance]]da beriladi.

Module privacy kontekstida `pub struct` fieldsni avtomatik public qilmaydi. Har bir field alohida `pub` bo'lmasa private by default qoladi.

## Syntax and Examples

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

Field access:

```rust
let email = user1.email;
```

Public/private fields:

```rust
pub struct Breakfast {
    pub toast: String,
    seasonal_fruit: String,
}
```

Bu yerda `toast` public field, `seasonal_fruit` private field.

## Common Mistakes

- Instance yaratishda barcha fields kerakligini unutish.
- Field access uchun tuple index syntax ishlatish.
- Struct instance immutable bo'lsa field assignment mumkin deb o'ylash.
- `pub struct` barcha fieldsni public qiladi deb o'ylash.
- Private field bor struct uchun public constructor yoki associated function kerak bo'lishini unutish.

## Related Concepts

- [[structs]]
- [[struct-instances|struct instances]]
- [[field-init-shorthand]]
- [[type-annotations|type annotations]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]

## Sources

- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
