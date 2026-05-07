---
title: "println! Macro"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, macros]
source_count: 3
---

# println! Macro

## Short Definition

`println!` terminalga text chiqaradigan standard Rust macro.

## Why It Matters

Hello World misoli Rust syntaxida macro call, string literal, va executable flow'ni ko'rsatadi.

## Mental Model

`println!` format string va argumentsni olib terminal output yaratadi.

`println!` odatda [[stdout]]ga yozadi. Struct debugging uchun `{}` emas, ko'pincha [[debug-formatting|Debug formatting]] (`{:?}` yoki `{:#?}`) kerak bo'ladi.

## Syntax and Examples

```rust
println!("Hello, world!");
```

## Common Mistakes

- `println` deb `!`siz yozish.
- Format placeholderlar sonini arguments bilan moslashtirmaslik.
- Custom structni `{}` bilan chiqarish uchun `Display` kerakligini unutish.

## Related Concepts

- [[macro]]
- [[main-function|main function]]
- [[hello-world]]
- [[display-formatting|Display formatting]]
- [[debug-formatting|Debug formatting]]
- [[stdout]]

## Sources

- [[1-2-hello-world-the-rust-programming-language]]
- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
