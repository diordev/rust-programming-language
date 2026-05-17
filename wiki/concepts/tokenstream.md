---
title: "TokenStream"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, proc-macro, compiler]
source_count: 1
---

# TokenStream

## Short Definition

Procedural macro input va output tipi. Rust source code'ining tokenlar ketma-ketligini ifodalaydi.

## Why It Matters

Procedural macro compiler bilan `TokenStream` orqali gaplashadi: input code tokenlarini oladi, generated code tokenlarini qaytaradi. `syn` va `quote` crate'lari shu tokenlar bilan ishlashni osonlashtiradi.

## Mental Model

```rust
pub fn macro_name(input: TokenStream) -> TokenStream
```

Input - macro qo'llangan joydagi tokenlar. Output - compiler keyin compile qiladigan generated Rust code.

## Syntax and Examples

```rust
use proc_macro::TokenStream;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();
    let generated = quote::quote! {
        // generated Rust code
    };
    generated.into()
}
```

## Common Mistakes

- `TokenStream`ni runtime data deb tushunish; bu compile-time macro API.
- Tokenlarni qo'lda string sifatida parse qilish; `syn` ko'p Rust syntaxini parse qila oladi.
- `quote!` outputini `.into()` qilmasdan `TokenStream` qaytarishga urinish.

## Related Concepts

- [[procedural-macros|procedural macros]]
- [[custom-derive-macros|custom derive macros]]
- [[attribute-like-macros|attribute-like macros]]
- [[function-like-macros|function-like macros]]
- [[compiler]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
