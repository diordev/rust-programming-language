---
title: "Debug Trait"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, traits, debug]
source_count: 1
---

# Debug Trait

## Short Definition

`Debug` trait developer-facing output uchun type value'sini debug formatda ko'rsatish capabilityini beradi.

## Why It Matters

Custom [[structs]] `println!("{rect:?}")` yoki [[dbg-macro|dbg! macro]] bilan chiqarilishi uchun `Debug` implement qilishi kerak. Ko'p hollarda bu [[derive-attribute|derive attribute]] orqali qilinadi.

## Mental Model

`Display` user-facing outputga o'xshaydi; `Debug` developerga internal state ko'rish uchun. Rust custom struct uchun output shaklini taxmin qilmaydi, shuning uchun explicit opt in talab qiladi.

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

## Common Mistakes

- `{:?}` ishlatish uchun `Debug` derive kerakligini unutish.
- `Display` va `Debug` formattingni bir xil deb o'ylash.

## Related Concepts

- [[traits]]
- [[derive-attribute|derive attribute]]
- [[debug-formatting|Debug formatting]]
- [[display-formatting|Display formatting]]
- [[structs]]

## Sources

- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
