---
title: "panic"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, errors]
source_count: 9
---

# panic

## Short Definition

`panic!` Rust program recover qilinmaydigan xato holatida current thread bajarilishini buzadigan runtime failure.

## Why It Matters

Rust ba'zi invalid operationsni memory unsafetyga aylantirmaydi; panic bug yoki broken invariant'ni ko'rinadigan qiladi.

## Mental Model

Panic "bu pathda normal davom etish to'g'ri emas" degani. Default javob [[stack-unwinding|stack unwinding]] yoki abort bo'lishi mumkin.

Expected failure uchun default vosita `[[result|Result]]`. Panic esa:

- broken invariant
- impossible state
- caller contract buzilishi
- davom ettirish xavfli bo'lgan holat

Advance panic source panic toolset'ni kengaytiradi:

- [[catch-unwind]]
- [[panic-any]]
- [[panic-hook]]
- [[todo-macro|todo!]]
- [[unimplemented-macro|unimplemented!]]
- [[unreachable-macro|unreachable!]]

Lekin bu vositalar panic'ni normal business error transportiga aylantirmaydi.

## Syntax and Examples

```rust
panic!("crash and burn");
```

```rust
let result = std::panic::catch_unwind(|| {
    panic!("boom");
});
```

## Common Mistakes

- User input yoki file/network failure'ni panic bilan ifodalash.
- `unwrap`ni odatiy error handlingga aylantirish.
- `catch_unwind` bor ekan, `Result` kerak emas deb o'ylash.

## Related Concepts

- [[result|Result]]
- [[unwrap]]
- [[panic-vs-result|panic! vs Result]]
- [[catch-unwind]]
- [[panic-any]]
- [[panic-hook]]
- [[backtrace]]
- [[never-type|never type (!)]]
- [[diverging-functions|diverging functions]]

## Sources

- [[wiki/sources/0-2-introduction]]
- [[3-2-data-types]]
- [[8-1-storing-lists-of-values-with-vectors]]
- [[wiki/sources/9-error-handling]]
- [[9-1-unrecoverable-errors-with-panic]]
- [[9-2-recoverable-errors-with-result]]
- [[9-3-to-panic-or-not-to-panic]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-panic]]

