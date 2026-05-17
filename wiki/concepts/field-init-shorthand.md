---
title: "Field Init Shorthand"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, structs, syntax]
source_count: 2
---

# Field Init Shorthand

## Short Definition

Field init shorthand field name va local variable/parameter name bir xil bo'lsa `field: field` o'rniga faqat `field` yozish syntax'i.

## Why It Matters

Struct instance yaratishda repetitionni kamaytiradi va constructor-like functionsni o'qishni osonlashtiradi.

## Mental Model

`email` yozuvi `email: email` degan ma'noni beradi, lekin faqat shu nomdagi variable scope'da mavjud bo'lsa. Bu pattern ko'pincha `new` yoki `build_*` kabi constructor-like functionsda uchraydi.

## Syntax and Examples

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

## Common Mistakes

- Field name va variable name farqli bo'lsa shorthand ishlaydi deb o'ylash.
- Shorthand fielddan oldin kerakli variable scope'da borligini unutish.

## Related Concepts

- [[structs]]
- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[functions]]

## Sources

- [[5-1-defining-and-instantiating-structs]]
- [[wiki/sources/rust-for-backend-developers-structs]]
