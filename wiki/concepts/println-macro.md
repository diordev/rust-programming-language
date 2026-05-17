---
title: "println! Macro"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, macros]
source_count: 5
---

# println! Macro

## Short Definition

`println!` terminalga text chiqaradigan standard Rust macro.

## Why It Matters

Hello World misoli Rust syntaxida macro call, string literal, va executable flow'ni ko'rsatadi.

## Mental Model

`println!` format string va argumentsni olib terminal output yaratadi.

`println!` odatda [[stdout]]ga yozadi. Struct debugging uchun `{}` emas, ko'pincha [[debug-formatting|Debug formatting]] (`{:?}` yoki `{:#?}`) kerak bo'ladi.

Hello Worldda `println!("Hello world!")` macro call syntaxini ham ko'rsatadi: `!` bu oddiy function call emasligini bildiradi.

## Syntax and Examples

```rust
println!("Hello, world!");
```

```rust
let magic_number = 5;
println!("Number is {}", magic_number);
println!("Number is {magic_number}");
println!("Number is {num}, square is {square}", num = magic_number, square = magic_number * magic_number);
```

`{}` odatda [[display-formatting|Display formatting]]ga tayanadi. Agar type `Display`ni implement qilmasa, ko'pincha `{:?}` bilan [[debug-formatting|Debug formatting]]ga o'tiladi.

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

- [[1-2-hello-world]]
- [[5-2-an-example-program-using-structs]]
- [[wiki/sources/rust-for-backend-developers-first-look]]
- [[wiki/sources/rust-for-backend-developers-console-output]]
