---
title: "Diverging Functions"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, functions, control-flow, types]
source_count: 2
---

# Diverging Functions

## Short Definition

Hech qachon callerga qaytmaydigan funksiya yoki expression. Return tipi `[[never-type|!]]`.

## Why It Matters

`panic!`, `todo!`, `unimplemented!`, `unreachable!`, `loop {}` kabi narsalar expression typing'ni tushunish uchun shu model kerak.

## Mental Model

`!` har tipga coerce bo'lgani uchun diverging expression boshqa type kutilayotgan joyda ishlay oladi.

## Syntax and Examples

```rust
fn explode() -> ! {
    panic!("end");
}
```

```rust
fn build() -> String {
    todo!()
}
```

## Common Mistakes

- `-> !` bo'lgan funksiya oxirida normal return kutish.
- Placeholder macro'lar oddiy return value deb o'ylash.

## Related Concepts

- [[never-type|never type (!)]]
- [[panic]]
- [[todo-macro|todo!]]
- [[unimplemented-macro|unimplemented!]]
- [[unreachable-macro|unreachable!]]

## Sources

- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-panic]]

