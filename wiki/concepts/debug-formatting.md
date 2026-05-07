---
title: "Debug Formatting"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, formatting, debug]
source_count: 1
---

# Debug Formatting

## Short Definition

Debug formatting `{:?}` yoki `{:#?}` placeholderlar orqali developer-facing representation chiqaradi.

## Why It Matters

Struct fieldsni debugging paytida ko'rish uchun [[debug-trait|Debug trait]] va debug formatting juda qulay.

## Mental Model

`{:?}` compact output, `{:#?}` pretty multiline output beradi. Bu output user-facing UI emas, developer inspection uchun.

## Syntax and Examples

```rust
println!("rect1 is {rect1:?}");
println!("rect1 is {rect1:#?}");
```

## Common Mistakes

- `Debug` derive qilmasdan `{:?}` ishlatish.
- Debug outputni stable user-facing format deb qabul qilish.

## Related Concepts

- [[debug-trait|Debug trait]]
- [[derive-attribute|derive attribute]]
- [[display-formatting|Display formatting]]
- [[dbg-macro|dbg! macro]]

## Sources

- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
