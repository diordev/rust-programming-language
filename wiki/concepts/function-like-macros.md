---
title: "Function-like Macros"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, proc-macro]
source_count: 1
---

# Function-like Macros

## Short Definition

Function callga o'xshab chaqiriladigan procedural macro turi: `name!(tokens...)`. Input tokenlarni parse qiladi va generated Rust code qaytaradi.

## Why It Matters

Function-like macro oddiy Rust expression bo'lmagan domain-specific syntaxni qabul qilishi mumkin. Masalan, `sql!(SELECT * FROM posts WHERE id=1)` SQL tokenlarini compile-time'da tekshirishi va Rust codega aylantirishi mumkin.

## Mental Model

```rust
sql!(SELECT * FROM posts WHERE id=1)
```

Bu oddiy function call emas. Parentheses ichidagi tokenlar Rust expression bo'lishi shart emas; macro ularni `TokenStream` sifatida oladi.

## Syntax and Examples

```rust
use proc_macro::TokenStream;

#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    // parse SQL-like tokens, generate Rust code
    input
}
```

## Common Mistakes

- Function-like macro argumentlari Rust expression bo'lishi shart deb o'ylash.
- `macro_rules!` function-like macro bilan bir xil deb o'ylash. Ikkalasi `name!(...)` ko'rinishi mumkin, lekin procedural version `TokenStream` ustida Rust code bilan ishlaydi.
- Macro compile-time'da ishlashini unutish.

## Related Concepts

- [[procedural-macros|procedural macros]]
- [[tokenstream|TokenStream]]
- [[declarative-macros|declarative macros]]
- [[attribute-like-macros|attribute-like macros]]
- [[procedural-macro-forms]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
