---
title: "Attribute-like and Function-like Procedural Macro Forms"
type: example
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, proc-macro]
source_count: 1
---

# Attribute-like and Function-like Procedural Macro Forms

## Context

Rust Book 20.5 custom derive'dan keyin procedural macro'larning qolgan ikki turini qisqa ko'rsatadi: attribute-like va function-like macros.

## Attribute-like Macro

User-facing syntax:

```rust
#[route(GET, "/")]
fn index() {
}
```

Macro definition:

```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
}
```

Meaning:

- `attr` - `GET, "/"` tokenlari.
- `item` - `fn index() {}` va function body tokenlari.

Attribute-like macro `derive`dan moslashuvchanroq: u faqat structs/enums bilan cheklanmaydi, function kabi itemlarga ham qo'llanishi mumkin.

## Function-like Macro

User-facing syntax:

```rust
let sql = sql!(SELECT * FROM posts WHERE id=1);
```

Macro definition:

```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
}
```

`sql!` parentheses ichidagi tokenlarni oladi. Bu tokenlar oddiy Rust expression bo'lishi shart emas; macro SQL-like syntaxni parse qilib, compile-time'da tekshirishi mumkin.

## Related Concepts

- [[procedural-macros|procedural macros]]
- [[attribute-like-macros|attribute-like macros]]
- [[function-like-macros|function-like macros]]
- [[tokenstream|TokenStream]]
- [[metaprogramming]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
