---
title: "Debug Trait"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, traits, debug]
source_count: 3
---

# Debug Trait

## Short Definition

`Debug` trait developer-facing output uchun type value'sini debug formatda ko'rsatish capabilityini beradi.

## Why It Matters

Custom [[structs]] `println!("{rect:?}")` yoki [[dbg-macro|dbg! macro]] bilan chiqarilishi uchun `Debug` implement qilishi kerak. Ko'p hollarda bu [[derive-attribute|derive attribute]] orqali qilinadi. Appendix C ko'rsatadigan yana bir muhim use case: `assert_eq!` fail bo'lsa qiymatlarni chiqarish uchun ham `Debug` kerak.

## Mental Model

`Display` user-facing outputga o'xshaydi; `Debug` developerga internal state ko'rish uchun. Rust custom struct uchun output shaklini taxmin qilmaydi, shuning uchun explicit opt in talab qiladi.

`derive` oilasi ichida `Debug` odatda eng birinchi qo'shiladigan trait bo'ladi: auto-derive source'lari bu traitning "developer-facing default" rolini aniq ko'rsatadi.

## Syntax and Examples

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

println!("rect1 is {rect1:?}");
println!("rect1 is {rect1:#?}");
```

`assert_eq!` bilan:

```rust
#[derive(Debug, PartialEq)]
struct Point { x: i32, y: i32 }
```

## Common Mistakes

- `{:?}` ishlatish uchun `Debug` derive kerakligini unutish.
- `Display` va `Debug` formattingni bir xil deb o'ylash.
- `assert_eq!` faqat `PartialEq` talab qiladi deb o'ylash.

## Related Concepts

- [[traits]]
- [[derive-attribute|derive attribute]]
- [[debug-formatting|Debug formatting]]
- [[display-formatting|Display formatting]]
- [[structs]]
- [[partial-eq]]
- [[test-macros|test macros (assert!, assert_eq!, assert_ne!)]]

## Sources

- [[5-2-an-example-program-using-structs]]
- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
- [[wiki/sources/22-3-c-derivable-traits|22.3]]
