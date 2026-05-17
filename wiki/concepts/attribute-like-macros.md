---
title: "Attribute-like Macros"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, attributes, proc-macro]
source_count: 1
---

# Attribute-like Macros

## Short Definition

Custom attribute yaratadigan procedural macro turi. `#[route(GET, "/")]` kabi item ustiga qo'yiladi.

## Why It Matters

Frameworklar function, struct, module kabi itemlarga domain-specific metadata va generated code qo'shishi mumkin. Web routing attributes, test attributes, serialization configurationlar shu uslubga yaqin.

## Mental Model

Attribute-like macro ikki token stream oladi:

- Attribute ichidagi tokenlar.
- Attribute biriktirilgan item tokenlari.

```rust
#[route(GET, "/")]
fn index() {}
```

Definition:

```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
}
```

`attr` = `GET, "/"`, `item` = `fn index() {}`.

## Syntax and Examples

```rust
use proc_macro::TokenStream;

#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
    // parse attr and item, then generate replacement item/code
    item
}
```

## Common Mistakes

- Attribute-like macro va derive macroni aralashtirish. `derive` structs/enums uchun, attribute-like macro ko'proq itemlarda ishlay oladi.
- `attr` va `item` tokenlarini adashtirish.
- Macro original itemni qaytarmasa, item yo'qolishi mumkinligini unutish.

## Related Concepts

- [[procedural-macros|procedural macros]]
- [[tokenstream|TokenStream]]
- [[derive-attribute|derive attribute]]
- [[function-like-macros|function-like macros]]
- [[procedural-macro-forms]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
