---
title: "unwrap"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, error-handling, result, option]
source_count: 6
---

# unwrap

## Short Definition

`unwrap` `Result` yoki `Option` ichidagi success value'ni qaytaradi, failure holatda esa [[panic]] qiladi.

## Why It Matters

U qisqa, lekin bu error handling emas. Failure bo'lsa program flow buziladi.

## Mental Model

`unwrap` "bu yerda value borligiga ishonaman; bo'lmasa crash qil" degani.

Prototype, examples, yoki testsda ba'zan maqbul. Production error handlingda esa ko'pincha `expect`, explicit `match`, yoki `?` yaxshiroq.

Advance panic source yana buni kuchaytiradi: `unwrap`dan kelgan panic'ni `catch_unwind` ushlashi mumkin, lekin bu `unwrap`ni normal error transportiga aylantirmaydi.

## Syntax and Examples

```rust
let a: Option<i32> = Some(5);
let n = a.unwrap();
```

## Common Mistakes

- User input, filesystem, network failure'larda `unwrap`ni default qilish.
- Uni `unsafe` bilan aralashtirish.
- Request boundary'da `unwrap` panicini keyin tutib olaman deb o'ylab, baribir API'ni panic-heavy qilish.

## Related Concepts

- [[expect]]
- [[result|Result<T, E>]]
- [[option|Option]]
- [[panic]]
- [[panic-vs-result|panic! vs Result]]
- [[catch-unwind]]
- [[never-type|never type (!)]]

## Sources

- [[9-2-recoverable-errors-with-result]]
- [[9-3-to-panic-or-not-to-panic]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-option]]
- [[wiki/sources/rust-for-backend-developers-result]]
- [[wiki/sources/rust-for-backend-developers-panic]]

