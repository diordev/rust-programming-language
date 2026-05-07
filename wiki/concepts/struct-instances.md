---
title: "Struct Instances"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, structs]
source_count: 1
---

# Struct Instances

## Short Definition

Struct instance struct type'ning concrete value'si bo'lib, har bir field uchun actual value saqlaydi.

## Why It Matters

Instance orqali program runtime data'ni named, checked shape bilan saqlaydi va uzatadi.

## Mental Model

Struct definition blueprint; instance shu blueprintdan yaratilgan value. Field order instance yaratishda definition orderiga bog'liq emas, chunki field names ishlatiladi.

## Syntax and Examples

```rust
let user1 = User {
    active: true,
    username: String::from("someusername123"),
    email: String::from("someone@example.com"),
    sign_in_count: 1,
};
```

Mutable instance:

```rust
let mut user1 = user1;
user1.email = String::from("anotheremail@example.com");
```

## Common Mistakes

- Faqat bitta field mutable bo'lishi mumkin deb o'ylash.
- Struct literal oxirgi expression sifatida return value bo'lishi mumkinligini unutish.

## Related Concepts

- [[structs]]
- [[struct-fields|struct fields]]
- [[variables-and-mutability|variables and mutability]]
- [[return-values|return values]]

## Sources

- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
