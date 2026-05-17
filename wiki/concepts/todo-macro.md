---
title: "todo!"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, macros, panic]
source_count: 1
---

# todo!

## Short Definition

`todo!` hali yozilmagan kod uchun placeholder macro bo'lib, panic qiladi.

## Why It Matters

Kutilayotgan type qanchalik murakkab bo'lmasin, vaqtincha placeholder qo'yish mumkin.

## Mental Model

`todo!` `!` qaytaradi, shu sabab compiler uni kerakli return type kontekstida qabul qiladi.

## Syntax and Examples

```rust
fn build() -> String {
    todo!("implement later")
}
```

## Common Mistakes

- Uni production path'da qoldirish.

## Related Concepts

- [[unimplemented-macro|unimplemented!]]
- [[unreachable-macro|unreachable!]]
- [[never-type|never type (!)]]

## Sources

- [[wiki/sources/rust-for-backend-developers-panic]]

