---
title: "HelloMacro Custom Derive Macro"
type: example
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, derive, proc-macro]
source_count: 1
---

# HelloMacro Custom Derive Macro

## Context

Rust Book Listings 20-37 through 20-42 custom derive procedural macro yaratishni ko'rsatadi. Maqsad: `#[derive(HelloMacro)]` yozilganda annotated type uchun `HelloMacro` trait implementationi generatsiya qilish.

## User Code

```rust
use hello_macro::HelloMacro;
use hello_macro_derive::HelloMacro;

#[derive(HelloMacro)]
struct Pancakes;

fn main() {
    Pancakes::hello_macro();
}
```

Expected output:

```text
Hello, Macro! My name is Pancakes!
```

## Trait Crate

```rust
pub trait HelloMacro {
    fn hello_macro();
}
```

Without macro, user har type uchun qo'lda yozishi kerak:

```rust
impl HelloMacro for Pancakes {
    fn hello_macro() {
        println!("Hello, Macro! My name is Pancakes!");
    }
}
```

## Derive Crate Setup

`hello_macro_derive/Cargo.toml`:

```toml
[lib]
proc-macro = true

[dependencies]
syn = "2.0"
quote = "1.0"
```

## Macro Implementation

```rust
use proc_macro::TokenStream;
use quote::quote;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();
    impl_hello_macro(&ast)
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    let name = &ast.ident;
    let generated = quote! {
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, Macro! My name is {}!", stringify!(#name));
            }
        }
    };
    generated.into()
}
```

## Explanation

- `#[proc_macro_derive(HelloMacro)]` user-facing `#[derive(HelloMacro)]` bilan bog'lanadi.
- `input: TokenStream` annotated item tokenlarini oladi.
- `syn::parse(input)` tokenlarni `DeriveInput` syntax tree'ga aylantiradi.
- `ast.ident` type nomini beradi (`Pancakes`).
- `quote!` generated Rust code template yaratadi.
- `#name` template ichida `name` tokenlarini qo'yadi.
- `stringify!(#name)` compile-time'da type nomini string literal qiladi.
- `.into()` `quote!` outputini `TokenStream`ga aylantiradi.

## Related Concepts

- [[custom-derive-macros|custom derive macros]]
- [[procedural-macros|procedural macros]]
- [[tokenstream|TokenStream]]
- [[derive-attribute|derive attribute]]
- [[traits]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
