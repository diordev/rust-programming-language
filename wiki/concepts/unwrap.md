---
title: "unwrap"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, error-handling, result, option]
source_count: 5
---

# unwrap

## Short Definition

`unwrap` `Result` yoki `Option` ichidagi success value'ni qaytaradigan, failure/none holatda [[panic|panic!]] qiladigan shortcut method.

## Why It Matters

`unwrap` prototyping, examples, yoki "bu yerda xato bo'lishi mumkin emas" assumptioni uchun qisqa. Lekin recoverable errors uchun u error handling emas; failure bo'lsa program to'xtaydi. Muhim aniqlik: `unwrap` Rust `unsafe` mexanizmi emas; u panic qilishi mumkin bo'lgan convenience shortcut.

## Mental Model

`unwrap` "value borligiga ishonaman; bo'lmasa crash qil" degani.

Examples, prototype code, va testsda `unwrap` temporary yoki deliberate panic marker sifatida mos bo'lishi mumkin. Production error handlingda esa ko'pincha [[expect]] yoki explicit `Result` handling yaxshiroq.

## Syntax and Examples

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap();
}
```

`File::open` `Err` qaytarsa, `unwrap` panic qiladi.

`Option` bilan:

```rust
let o: Option<i32> = Some(5);
let i: i32 = o.unwrap();
```

## Common Mistakes

- User input, file system, network kabi normal failure holatlarida `unwrap`ni odatiy yechim qilish.
- `unwrap` va [[expect]] orasidagi debugging message farqini hisobga olmaslik.
- `unwrap` errorni handle qiladi deb o'ylash; u panic qiladi.
- Prototype markerini production code'da qoldirish.
- `unwrap`ni `unsafe` deb atash.

## Related Concepts

- [[expect]]
- [[result|Result<T, E>]]
- [[option|Option]]
- [[panic|panic!]]
- [[recoverable-errors|recoverable errors]]
- [[panic-vs-result|panic! vs Result]]
- [[testing]]
- [[never-type|never type (!)]] — `panic!` `!` qaytaradi → `match` da `T` ga coerce
- [[diverging-functions|diverging functions]]

## Sources

- [[9-2-recoverable-errors-with-result]]
- [[9-3-to-panic-or-not-to-panic]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-option]]
- [[wiki/sources/rust-for-backend-developers-result]]
