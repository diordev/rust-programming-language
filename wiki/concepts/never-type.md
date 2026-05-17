---
title: "Never Type (!)"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, types, control-flow]
source_count: 4
---

# Never Type (`!`)

## Short Definition

`!` qiymatga ega bo'la olmaydigan empty type. Hech qachon normal qaytmaydigan expression yoki functionni ifodalaydi.

## Why It Matters

`panic!`, `loop {}`, `process::exit`, `todo!`, `unimplemented!`, `unreachable!` kabi constructlar typing qoidalarini aynan shu type bilan oladi.

## Mental Model

`!` "shu nuqtadan keyin control normal yo'l bilan qaytmaydi" degani. Shu sabab u boshqa kutilgan type'ga coerce bo'ladi.

## Syntax and Examples

```rust
fn forever() -> ! {
    loop {}
}
```

```rust
fn value() -> String {
    todo!()
}
```

## Common Mistakes

- `!`ni unit type `()` bilan aralashtirish.
- Placeholder macro'lar `String` yoki boshqa value'ni "haqiqatan" qaytaradi deb o'ylash.

## Related Concepts

- [[panic]]
- [[diverging-functions|diverging functions]]
- [[todo-macro|todo!]]
- [[unimplemented-macro|unimplemented!]]
- [[unreachable-macro|unreachable!]]
- [[unit-type|unit type]]

## Sources

- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-primitive-types]]
- [[wiki/sources/rust-for-backend-developers-functions]]
- [[wiki/sources/rust-for-backend-developers-panic]]

