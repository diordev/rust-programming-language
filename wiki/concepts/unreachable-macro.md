---
title: "unreachable!"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, macros, panic]
source_count: 1
---

# unreachable!

## Short Definition

`unreachable!` shu branchga kelinmasligi kerak degan assertion macro.

## Why It Matters

Impossible state yoki caller contract'i buzilgan branchni aniq belgilaydi.

## Mental Model

Technically branchga kelish mumkin, lekin design bo'yicha kelinmasligi kerak. Kelsa panic qiladi.

## Syntax and Examples

```rust
match version {
    1 => handle_v1(),
    2 => handle_v2(),
    _ => unreachable!("unknown api version"),
}
```

## Common Mistakes

- Validation qilinmagan input branch'ida ishlatish.

## Related Concepts

- [[panic]]
- [[todo-macro|todo!]]
- [[never-type|never type (!)]]
- [[function-contracts|function contracts]]

## Sources

- [[wiki/sources/rust-for-backend-developers-panic]]
